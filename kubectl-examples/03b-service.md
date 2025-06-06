# Example Service 

## Schritt 1: Deployment 

```
cd
mkdir -p manifests
cd manifests 
mkdir 04-service 
cd 04-service 
#vi 01-deploy.yml 
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 7 # tells deployment to run 8 pods matching the template
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
kubectl apply -f .
```

## Schritt 2:

```
nano 02-svc.yml
```

```
apiVersion: v1
kind: Service
metadata:
  name: svc-nginx
spec:
  type: ClusterIP
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: nginx
```

```
kubectl apply -f . 
```

## Schritt 3 Endpunkte gefunden ?

```
kubectl get svc svc-nginx
kubectl describe svc svc-nginx
```

```
kubectl get endpoints svc-nginx
kubectl get endpoints svc-nginx -o yaml 
```

## Schritt 4 Löschen deployment und prüfen die Endpunkte 

```
kubectl delete -f deploy.yml
# Uups Endpunkte sind weg 
kubectl describe svc svc-nginx 
```

```
kubectl apply -f .
# Endpunkte sind wieder da, aber neue POD-IPs
kubectl describe svc svc-nginx 
```

## Ref.

  * https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/
