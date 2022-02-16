# Example: static pod nginx with 80 exposed 

## What is containerPort (from kubectl explain) ?

```
containerPort        <integer> -required-
     Number of port to expose on the pod's IP address. This must be a valid port
     number, 0 < x < 65536.
     
```

## Walkthrough 

```
vi nginx-static-expose.yml 

apiVersion: v1
kind: Pod
metadata:
  name: nginx-static-web
  labels:
    webserver: nginx
spec:
  containers:
  - name: web
    image: nginx
    ports:
    - name: web
      containerPort: 80
      protocol: TCP

```

```
kubectl apply -f nginx-static-expose.yml 
kubectl describe nginx-static-web 
# show config 
kubectl get pod/nginx-static-web -o yml 
```
