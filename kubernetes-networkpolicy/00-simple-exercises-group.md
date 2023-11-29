# Simple Network Policy Example (for group in 1 cluster)

## Schritt 1: Deployment und Service erstellen 

```
KURZ=jm
kubectl create ns policy-demo-$KURZ 
```

```
cd 
mkdir -p manifests
cd manifests
mkdir np
cd np
```

```
# nano 01-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 8
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.23
        ports:
        - containerPort: 80
```

```
kubectl -n policy-demo-$KURZ apply -f . 
```





```
# Hier bitte Euer Kürzel eintragen 
KURZ=jm
```

```

kubectl expose --namespace=policy-demo-$KURZ deployment nginx --port=80
# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo-$KURZ access --rm -ti --image busybox
```

```
# innerhalb der shell 
wget -q nginx -O -
```

```
# Schritt 2: Policy festlegen, dass kein Ingress-Traffic erlaubt
# in diesem namespace: policy-demo-$KURZ 
kubectl create -f - <<EOF
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: default-deny
  namespace: policy-demo-$KURZ
spec:
  podSelector:
    matchLabels: {}
EOF
# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo-$KURZ access --rm -ti --image busybox
```

```
# innerhalb der shell 
wget -q nginx -O -
```

```
# Schritt 3: Zugriff erlauben von pods mit dem Label run=access 
kubectl create -f - <<EOF
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: access-nginx
  namespace: policy-demo-$KURZ
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
    - from:
      - podSelector:
          matchLabels:
            run: access
EOF

# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
# pod hat durch run -> access automatisch das label run:access zugewiesen 
kubectl run --namespace=policy-demo-$KURZ access --rm -ti --image busybox
```

```
# innerhalb der shell 
wget -q nginx -O -
```

``` 
kubectl run --namespace=policy-demo-$KURZ no-access --rm -ti --image busybox
```

```
# in der shell  
wget -q nginx -O -
```

```

kubectl delete ns policy-demo-$KURZ 

```


## Ref:

  * https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-basic
