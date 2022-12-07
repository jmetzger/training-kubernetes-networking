# Taints and Tolerations 

## Taints 

```
Taints schliessen auf einer Node alle Pods aus, die nicht bestimmte label haben:

Möglichkeiten:

o Sie werden nicht gescheduled - NoSchedule 
o Sie werden nicht executed - NoExecute 
o Sie werden möglichst nicht gescheduled. - PreferNoSchedule 

```

## Tolerations 

```
Tolerations werden auf Pod-Ebene vergeben: 
tolerations: 

Ein Pod kann (wenn es auf einem Node taints gibt), nur 
gescheduled bzw. ausgeführt werden, wenn er die 
Labels hat, die auch als
Taints auf dem Node vergeben sind.
```

## Taints vergeben 

```
# Dieses labels muss ein pod haben -> gpu:true 
# Ansonsten wird er nicht gescheduled 
kubectl taint nodes n1 gpu=true:NoSchedule

```

## Pod mit richtigen Tolerations schedulen 

``` 
cd 
mkdir -p manifests
cd manifests 
mkdir tainttest 
cd tainttest 
nano 01-tolerations.yml
```


```
# vi 01-tolerations-pod.yml 
apiVersion: v1
kind: Pod
metadata:
  name: nginx-test
  labels:
    env: test-env
spec:
  containers:
  - name: nginx
    image: nginx:latest
    imagePullPolicy: IfNotPresent
    resources:
      requests:
        memory: "128Mi"
        cpu: "250m"
      limits:
        memory: "512Mi"
        cpu: "500m"
  tolerations:
  - key: "gpu"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"

```

```
kubectl apply -f .
# but it does not mean, it will really be scheduled on first node 
# only that it can be scheduled here 
kubectl get pods -o wide 

kubectl delete -f .
```


## Pod mit mit der falschen Toleration schedulen 

```
# Damit wir sehen, ob das wirklich funktioniert, drainen wir zunächst die anderen Nodes 
kubectl drain n11
kubectl drain n111
```

```
# vi 02-notolerations.yml 
apiVersion: v1
kind: Pod
metadata:
  name: nginx-test-wrong-tol
  labels:
    env: test-env
spec:
  containers:
  - name: nginx
    image: nginx:latest
    imagePullPolicy: IfNotPresent
    resources:
      requests:
        memory: "128Mi"
        cpu: "250m"
      limits:
        memory: "512Mi"
        cpu: "500m"
  tolerations:
  # hier cpu statt gpu 
  - key: "cpu"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"

```

```
# er dürfte jetzt nicht gescheduled werden, 
weil es keine node für ihn gibt 
kubectl apply -f . 
kubectl get describe pods nginx-test-wrong-tol

```



## Taints rausnehmen 

```
kubectl taint nodes n1 gpu:true:NoSchedule-

```

## References 
  
  * [Doku Kubernetes Taints and Tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)
  * https://blog.kubecost.com/blog/kubernetes-taints/
