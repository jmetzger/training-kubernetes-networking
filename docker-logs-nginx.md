# Docker logs (Nginx) 

## Allgemein 
```
# Erstmal nginx starten und container-id wird ausgegeben 
docker run -d nginx 
a234
docker logs a234 # a234 sind die ersten 4 Ziffern der Container ID 
```

## Laufende Log-Ausgabe 

```
docker logs -f a234 
# Abbrechen CTRL + c 
```
