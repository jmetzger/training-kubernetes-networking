# tcpdump - nodeport example 

## Schritt 1: Ausgangsbasis 

```
# Basis 03-deploy
cd
cd manifests
cd 03-deploy
ls -la 
```

## Schritt 2: Wir passen den Service an und Anzahl der Replicas in Deploy 

```
nano 02-svc.yml
```

```
# Ändern Zeile mit type:
# Ändern in type: NodePort 
```

```
nano deploy.yml
```

```
# Ändern Zeile mit replicas:
# Ändern in replicas: 1
```

```
kubectl apply -f .
```

## Schritt 2: Daten sammeln 

```
kubectl get svc svc-nginx 
# Beispiel -> 32682 
# Worker öffentliche IP rausfinden
kubectl get node -o wide | grep worker1
# worker1 -> 164.92.131.128
```

## Schritt 3: 2. Session öffnen 

```
# ein debug-pod auf worker1 starten 
kubectl debug -it node/worker1 --image nicolaka/netshoot 
.# im pod tcpdump auf port von Schritt 2
tcdump -i eth0 port 32682  
```

## Schritt 4: 1. Session 

```
# while kontinuierlich 
curl http://164.92.131.128:32682
```

## Schritt 5: 2. Session ausgabe tcpdump




## Optional: Schritt 6

```
Welcher Pod von nginx -> daneben einen debug container starten
kubectl get pods 
kubectl debug -it nginx-deployment-5948f7484f-sbq9v --image nicolaka/netshoot 
```

