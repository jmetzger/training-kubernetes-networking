# Grafana Dashboard 

## Status

  * Wir haben bereits mit den Addons Grafana ausgerollt,
  * Dieses wollen wir jetzt aktivieren

## Schritt 1: Dashboard aktivieren -> achtung jeder nimmt seinen eigenen Port 

```
# um Grunde macht das auch nur ein port - forward 
# Das macht der Trainer nur 1x, dann können alle dort zugreifen
istioctl dashboard grafana --port=3000 --browser=false
```

```
# Jetzt über den Browser öffnen
http://localhost:3000
# Dann Dashboard -> istio -> istio services
```

```
# Lass uns mal Traffic hinschicken vom Client aus
# ip vom ingressgateway from loadBalancer 
while true; do curl http://jochen.istio.t3isp.de/api/catalog; sleep .5; done 

# Und das das Dashboard nochmal refreshend
#-> General ausklappen 
```
