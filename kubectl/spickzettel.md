# Wichtige kubectl kommandos 

## Allgemein 

```
# Zeige Information über das Cluster 
kubectl cluster-info 

# Welche api-resources gibt es ?
kubectl api-resources 

# Hilfe zu object und eigenschaften bekommen
kubectl explain pod 
kubectl explain pod.metadata
kubectl explain pod.metadata.name 

```

## Arbeiten mit manifesten 

```
kubectl apply -f nginx-replicaset.yml 
# Wie ist aktuell die hinterlegte config im system
kubectl get -o yaml -f nginx-replicaset.yml 

# Änderung in nginx-replicaset.yml z.B. replicas: 4 
# dry-run - was wird geändert 
kubectl diff -f nginx-replicaset.yml 

# anwenden 
kubectl apply -f nginx-replicaset.yml 

# Alle Objekte aus manifest löschen
kubectl delete -f nginx-replicaset.yml 


```

## Ausgabeformate 

```
# Ausgabe kann in verschiedenen Formaten erfolgen 
kubectl get pods -o wide # weitere informationen 
# im json format
kubectl get pods -o json 

# gilt natürluch auch für andere kommandos
kubectl get deploy -o json 
kubectl get deploy -o yaml 

# get a specific value from the complete json - tree 
kubectl get node k8s-nue-jo-ff1p1 -o=jsonpath='{.metadata.labels}'

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
# Zeige labels der Pods
kubectl get pods --show-labels 

# Pods aus allen Namespaces anzeigen
kubectl get pods -A

# Zeige pods mit einem bestimmten label 
kubectl get pods -l app=nginx 

# Status eines Pods anzeigen 
kubectl describe pod nginx 

# Pod löschen 
kubectl delete pod nginx 

# Kommando in pod ausführen 
kubectl exec -it nginx -- bash 
# direkt in den 1. Pod des Deployments wechseln
kubectl exec -it deployment/name-des-deployments -- bash 

```

## Logs ausgeben 

```
kubectl logs podname
# -n = namespace
# | less -> seitenweise Ausgabe
kubectl -n ingress logs nginx-ingress-ingress-nginx-controller-7bc7c7776d-jpj5h | less
```

## Arbeiten mit namespaces 

```
# Welche namespaces auf dem System 
kubectl get ns 
kubectl get namespaces 
# Standardmäßig wird immer der default namespace verwendet 
# wenn man kommandos aufruft 
kubectl get deployments 

# Möchte ich z.B. deployment vom kube-system (installation) aufrufen, 
# kann ich den namespace angeben
kubectl get deployments --namespace=kube-system 
kubectl get deployments -n kube-system 

# wir wollen unseren default namespace ändern 
kubectl config set-context --current --namespace <dein-namespace>
```



## Referenz

  * https://kubernetes.io/de/docs/reference/kubectl/cheatsheet/
