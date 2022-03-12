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



