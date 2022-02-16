# Wichtige kubectl kommandos 

## Allgemein 

```
# Zeige Information über das Cluster 
kubectl cluster-info 

# Hilfe zu object und eigenschaften bekommen
kubectl describe pod 
kubectl describe pod.metadata
kubectl describe pod.metadata.name 

```

## Zu den Pods 

```
# Start einen pod // BESSER: direkt manifest verwenden
# kubectl run podname image=imagename 
kubectl run nginx image=nginx 

# Pods anzeigen 
kubectl get pods 
kubectl get pod
# Format weitere Information 
kubectl get pod -o wide 


# Status eines Pods anzeigen 
kubectl explain pod nginx 

# Pod löschen 
kubectl delete pod nginx 

# Kommando in pod ausführen 
kubectl exec -it nginx -- bash 

```
