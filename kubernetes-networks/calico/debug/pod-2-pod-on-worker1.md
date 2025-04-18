# Assigning pods to nodes 

## Walkthrough 

```
# leave worker3 as is 
kubectl label nodes worker1 machine=worker1
kubectl label nodes worker2 machine=worker2
kubectl get nodes --show-labels
```

## 1. Deployment auf worker1

```
cd
mkdir -p manifests
cd manifests
mkdir calicotest
cd calicotest
```

```
nano 01-deploy.yml
```

```
# nginx-deployment 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment-calicotest
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 1 
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
      nodeSelector:
        machine: worker1
```

## 2. noch ein Pod auf worker1 

```
nano 02-pod.yaml
```

``` 
apiVersion: v1
kind: Pod
metadata:
  name: nginx-calicotest
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    machine: worker1
```



```
kubectl apply -f .
```
## 3. Find out about cali-interfaces 

```
# Ip von ziel rausfinden 
calicoctl get wep
```

```
kubectl debug -it node/worker1 --image=busybox  
```

```
# Example 
# nginx-calicotest (192.168.235.141) -> ngin-deployment-calicotest.....z6rmp (192.168.235.138)
# Now grep for the destination 
route -n | grep 192.168.235.138 
```
