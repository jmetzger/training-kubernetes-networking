# Feste IP aus Pool beziehen (metallb) 

## Beispiel 

```
cd
mkdir -p manifests/lb/
```

```
# Service yaml anpassen
nano 03-service.yaml
```

```
apiVersion: v1
kind: Service
metadata:
  name: svc-nginx
  labels:
    svc: nginx
spec:
  type: LoadBalancer
# eure ip aus dem pool nehmen 
  loadBalancerIP: 167.99.130.85
  ports:
  - port: 80
    protocol: TCP
  selector:
    run: web-nginx
```
