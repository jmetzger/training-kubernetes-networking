# 2 Pods on 2 nodes 

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
