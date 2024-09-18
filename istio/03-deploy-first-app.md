# Deploy first app (Exercise for group) 

## Overview (what we want to do) 

![image](https://github.com/jmetzger/training-kubernetes-advanced/assets/1933318/285fc65a-57ec-425f-bcd7-729777f79a7d)

  * Catalog Service is reachable through api

## Step 1: Vorbereitung - repo mit beispielen klonen 

```
cd
git clone https://github.com/jmetzger/istio-exercises/
cd istio-exercises 
```

## Step 2: Eigenen Namespace erstellen 

```
# Jeder Teilnehmer erstellt seinen eigenen Namespace
# z.B. istioapp-tlnx
# d.h. für Teilnehmer 5 (tln5) -> istioapp-tln5
kubectl create ns istioapp-tln5 
# Context so einstellen, dass dieser namespace verwendet
kubectl config set-context --current --namespace istioapp-tln5 
```

## Step 3: Anwendung untersuchen / istioctl kube-inject 

  * Ihr könnt unten direkt den Pfad nehmen, das ist einfacher ;o) 

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: catalog
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: catalog
  name: catalog
spec:
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 3000
  selector:
    app: catalog
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: catalog
    version: v1
  name: catalog
spec:
  replicas: 1
  selector:
    matchLabels:
      app: catalog
      version: v1
  template:
    metadata:
      labels:
        app: catalog
        version: v1
    spec: 
      serviceAccountName: catalog
      containers:
      - env:
        - name: KUBERNETES_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        image: istioinaction/catalog:latest
        imagePullPolicy: IfNotPresent
        name: catalog
        ports:
        - containerPort: 3000
          name: http
          protocol: TCP
        securityContext:
          privileged: false
```

```
# schauen wir uns das mal mit injection an 
istioctl kube-inject -f services/catalog/kubernetes/catalog.yaml | less 
```

## Step 4: Automatische Injection einrichten. 

```
# kubectl label namespace istioapp-tlnx istio-injection=enabled 
# z.B
kubectl label namespace istioapp-tln1 istio-injection=enabled 
```

## Step 5: catalog ausrollen 

```
kubectl apply -f services/catalog/kubernetes/catalog.yaml
```

```
# Prüfen, ob wirklich 2 container in einem pod laufen,
# dann funktioniert die Injection
# WORKS, Yeah !
kubectl get pods 
```

## Step 6: Wir wollen den Catalog jetzt erreichen 

```
# do it from your namespace, e.g. tlnx 
# z.B. 
kubectl -n tln1 run -it --rm curly --image=curlimages/curl -- sh
```

```
# within shell of that pod
# catalog.yourappnamespace/items/1
curl http://catalog.istioapp-tln1/items/1
exit
```

## Step 7: Jetzt deployen wir die webapp 

```
# Wir schauen uns das manifest für die webapp an
# und ändern die env-variablen CATALOG_SERVICE_HOST 
# tlnx durch Eure Teilnehmernummer ersetzen 
catalog.istioapp-tlnx  
```

```
kubectl apply -f services/webapp/kubernetes/webapp.yaml 
kubectl get pod
```

## Step 8: Verbindung zu webapp testen 

```
# tlnx
# kubectl -n tlnx run -it --rm curly --image=curlimages/curl -- sh
# z.B. 
kubectl -n tln5 run -it --rm curly --image=curlimages/curl -- sh
```

```
# Within shell connect to webapp
curl -s http://webapp.istioapp-tln1/api/catalog/items/1
exit
```

```
# Wir können es aber auch visualisieren
kubectl port-forward deploy/webapp 8001:8080
# z.B. Teilnehmer tln1 -> 8001:8080

# WICHTIG Jeder Teilneher sollte hier einen abweichenden Port nehmen 
# Jetzt lokal noch einen Tunnel aufbauen
# s. Anleitung Putty
# Source Port: 8080 # das ist der auf dem Rechner 
# Destination: localhost:8001
# Add
# Achtung -> danach noch Session speichern
```

```
# Jetzt im Browser http://localhost:8080
# aufrufen
```

## Step 9: Ingress - Gateway konfigurieren (ähnlich wie Ingress-Objekt) 

```
# wir schauen uns das vorher mal an 
```

```
# namespace - fähig, d.h. ein Gateway mit gleichem Namen pro Namespace möglich 
cat ingress-virtualservice/ingress-gateway.yaml
# hier bitte bei Hosts hostname eintragen, der für t3isp.de verwendet, und zwar
# jeder Teilnehmer eine eigene Subdomain:  z.B. jochen.istio.t3isp.de 
kubectl apply -f ingress-virtualservice/ingress-gateway.yaml
```

## Step 10: Reach it from outside 

```
# We need to find the loadbalancer IP
kubectl -n istio-system get svc 
# in unserem Fall
146.190.177.12
# Das trägt Jochen dns t3isp.de ein. 

# Wir können jetzt also das System von extern erreichen
# vomn client aus, oder direkt über den Browser 
#curl -i 146.190.177.12/api/catalog/items/1
# Hier hostname statt ip einträgen
curl -i http://tlnx.istio.t3isp.de/api/catalog/items/1 

```

```
# Wir können auch über istioctl direkt überprüfen, ob es einen Routen-Config gibt
istioctl proxy-config routes deploy/istio-ingressgateway.istio-system

# Falls das nicht funktioniert, können wir auch überprüfen ob ein gateway und ein virtualservice installiert wurde
kubectl get gateway
kubectl get virtualservice
# Kurzform des Services reicht, weil im gleichen namespace
# Wo soll es hingehen -> == -> Upstream 
# route -> destination -> host -> webapp 
kubectl get virtualservice -o yaml 
```

```
## Wichtiger Hinweis, auf beiden Seiten ingressgateway und vor dem Pod des Dienstes Webapp
## Sitzt ein envoy-proxy und kann Telemetrie-Daten und Insight sammeln was zwischen den
## applicationen passiert -> das passiert über ein sidecar in jeder Applikation 

## Wichtig: Das passiert alles ausserhalb der Applikation
## Nicht wie früher z.B. bei Netflix innerhalb z.B. für die Sprache Java
```
