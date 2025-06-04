# Routing by http-header 

## Step 1: Kong installieren 

```
helm repo add kong https://charts.konghq.com
helm -n kong upgrade --install kong kong/ingress --version 0.19.0 --create-namespace
kubectl get ingressclasses
# IP - Addresse 
kubectl -n kong get svc 
```


## Step 2: Config einrichten / Services sind bereits da ! 

```
nano ingress-kong.yaml 
```

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-bar
  annotations:
    konghq.com/headers.x-version: "bar"
spec:
  ingressClassName: kong
  rules:
  - host: app.<dein-name>.t3isp.de
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: bar
            port:
              number: 80
---
# Default (ohne Header)
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-foo
spec:
  ingressClassName: kong
  rules:
  - host: app.<dein-name>.t3isp.de
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: foo  # Fallback
            port:
              number: 80


```

```
kubectl apply -f .
```

## Step 3: Testen 

```
curl http://app.<dein-name>.t3isp.de
curl -H "x-version: bar" http://app.<dein-name>.t3isp.de
```
