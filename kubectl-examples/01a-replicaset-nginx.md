# Replicaset

```
cd
mkdir -p manifests
cd manifests
mkdir 02-rs 
cd 02-rs 
vi rs.yml
```

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replica-set
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      name: template-nginx-replica-set
      labels:
        tier: frontend
    spec:
      containers:
        - name: nginx
          image: nginx:1.21
          ports:
            - containerPort: 80
             

             
```

```
kubectl apply -f rs.yml 
```
