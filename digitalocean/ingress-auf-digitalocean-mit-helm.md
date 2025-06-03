# Ingress Controller - Feature aufsetzen mit Helm auf Digitalocean 

## Basics 

  * Das Verfahren funktioniert auch so auf anderen Plattformen, wenn helm verwendet wird und noch kein IngressController vorhanden
  * Ist kein IngressController vorhanden, werden die Ingress-Objekte zwar angelegt, es funktioniert aber nicht. 

## Prerequisites 

  * kubectl und helm muss eingerichtet sein 

## Walkthrough (Setup Ingress Controller) 

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
# Falls repo bereits mit add hinzugefügt war 
# helm repo update

helm install nginx-ingress ingress-nginx/ingress-nginx --namespace ingress --create-namespace --version 4.12.2

# See when the external ip comes available
kubectl -n ingress get all
kubectl -n ingress get svc
```


```
# Output  
NAME                                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                      AGE     SELECTOR
nginx-ingress-ingress-nginx-controller   LoadBalancer   10.245.78.34   157.245.20.222   80:31588/TCP,443:30704/TCP   4m39s   app.kubernetes.io/component=controller,app.kubernetes.io/instance=nginx-ingress,app.kubernetes.io/name=ingress-nginx

# Now setup wildcard - domain for training purpose 
# inwx.com
*.lab1.t3isp.de A 157.245.20.222 


```

