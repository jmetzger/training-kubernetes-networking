# Example Service 

## Schritt 1: Deployment 

```
cd
mkdir -p manifests
cd manifests 
mkdir 04-service 
cd 04-service 
vi 01-deploy.yml 
```

```
# 01-deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
spec:
  selector:
    matchLabels:
      run: my-nginx
  replicas: 3
  template:
    metadata:
      labels:
        run: my-nginx
    spec:
      containers:
      - name: my-nginx
        image: nginx
        ports:
        - containerPort: 80
```

```
kubectl apply -f .
```

## Schritt 2:


```
# 02-svc.yml 
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    svc: nginx
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    run: my-nginx
```

```
kubectl apply -f . 
```


## Ref.

  * https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/
