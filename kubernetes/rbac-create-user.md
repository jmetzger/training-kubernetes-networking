# RBAC - Create user for kubeconfig with restricted permissions (microk8s) 

## Enable RBAC in microk8s 

```
# This is important, if not enable every user on the system is allowed to do everything 
microk8s enable rbac 
```

## Schritt 1: Nutzer-Account auf Server anlegen / in Client 

```
cd 
mkdir -p manifests/rbac
cd manifests/rbac
```

###  Mini-Schritt 1: Definition für Nutzer 

```
# vi service-account.yml 
apiVersion: v1
kind: ServiceAccount
metadata:
  name: training
  namespace: default


kubectl apply -f service-account.yml 
```


### Mini-Schritt 2: ClusterRolle festlegen - Dies gilt für alle namespaces, muss aber noch zugewiesen werden

```
## Bevor sie zugewiesen ist, funktioniert sie nicht - da sie keinem Nutzer zugewiesen ist 

# vi pods-clusterrole.yml 
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: pods-clusterrole
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

kubectl apply -f pods-clusterrole.yml 
```




