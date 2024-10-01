# my traceroute 

## Setup 

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-worker1
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    kubernetes.io/hostname: worker1
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx-worker2
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    kubernetes.io/hostname: worker2
---
```

## my traceroute 

```
kubectl debug -it node/worker1 --image nicolaka/netshoot
```

```
mtr <ip-addresse-des-pod-auf-worker2>
# ohne dns auflösung
mtr -n <ip-addresse-des-pod-auf-worker2>
```
