# Verbindung zu pod testen 

## Situation 

```
Managed Cluster und ich kann nicht auf einzelne Nodes per ssh zugreifen
```

## Behelf: Eigenen Pod starten mit busybox 

```
# laengere Version 
kubectl run podtest --rm -ti --image busybox -- /bin/sh
# kuerzere Version 
kubectl run podtest --rm -ti --image busybox 
```

## Example test connection 

```
# wget befehl zum Kopieren
wget -O - http://10.244.0.99
```

```
# -O -> Output (grosses O (buchstabe)) 
kubectl run podtest --rm -ti --image busybox -- /bin/sh
/ # wget -O - http://10.244.0.99
/ # exit 
```
