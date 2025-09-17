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


## Schritt 5 : NodePort : Short version 

```
nano ß2-svc.yml
# in Zeile type: 
# ClusterIP ersetzt durch NodePort 

kubectl apply -f .
kubectl get svc
kubectl get nodes -o wide
# im client 
curl http://164.92.193.245:30280
```


## Schritt 6 : Service mit LoadBalancer (ExternalIP)

```
nano 02-svc.yml
# in Zeile type: 
# NodePort ersetzt durch LoadBalancer  

kubectl apply -f .
kubectl get svc svc-nginx
kubectl describe svc svc-nginx 
kubectl get svc svc-nginx -w 
# spätestens nach 5 Minuten bekommen wir eine externe ip
# z.B. 41.32.44.45

curl http://41.32.44.45 
```


## Optional: Example getting a specific ip from loadbalancer (if supported) 

```
apiVersion: v1
kind: Service
metadata:
  name: svc-nginx2
spec:
  type: LoadBalancer
  # this line to get a specific ip if supported
  loadBalancerIP: 10.34.12.34
  ports:
  - port: 80
    protocol: TCP
  selector:
    web: my-nginx
```       

## Ref.

  * https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/
