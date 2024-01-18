# hubble ui 

## Variante A: wenn kubectl/cilium/hubble auf Client 

```
hubble  ui 
```

## Variante B: jump-server ohne browser

### Auf dem Jump-Server Port-Forwarding aktivieren 

```
# dadurch auf Jump-Server wenn es dort einen Browser gäbe: http://localhost:30080
kubectl -n kube-system port-forward svc/hubble-ui 30080:80 --address='0.0.0.0' von extern notwendig 
```


### Vom Lokalen - Rechner  

#### Schritt 1. Variante A 

```
# Lokal port 8080 
ssh -L 8080:161.35.64.88:30080 tlnX@161.35.64.88
```

#### Schritt 1: Variante B 

```
Für Putty Nutzer:
Unter SSH Tunnels gehen, Source Port: 8080, Destination: 161.35.64.88:30080
Auf Add klicken
```

#### Schritt 2: Lokal im Browser öffnen 

```
http://localhost:30080
```
