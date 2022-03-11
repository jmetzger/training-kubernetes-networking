# RBAC - Create user

## Enable RBAC in microk8s 

```
microk8s enable rbac 

```

## Schritt 1: Systemaccount anlegen und in kubeconfig hinterlegen 

```
kubectl create -n kube-system  serviceaccount training
# extract name of the token from here 
kubectl get -n kube-system  serviceaccount training -o yaml

# Secret auslesen und base64 dekodieren 
# $() geht nur in der bash 
TOKEN=$(kubectl get -n kube-system secret training-token-nxdfl -o jsonpath='{.data.token}' | base64 --decode)
echo $TOKEN
kubectl config set-credentials training --token=$TOKEN
# trainingc vorher in .kube/config eingetragen 
kubectl config use-context trainingc

# Hier reichen die Rechte nicht aus 
kubectl get pods
# Error from server (Forbidden): pods is forbidden: User "system:serviceaccount:kube-system:training" cannot list # resource "pods" in API group "" in the namespace "default"
```

## Schritt 2: Role und Rolebinding festlegen 

```
cd 
mkdir -p manifests/rbac
vi namespace.json
##  Minischritt 1:
{
  "apiVersion": "v1",
  "kind": "Namespace",
  "metadata": {
    "name": "training",
    "labels": {
      "name": "training"
    }
  }
}

kubectl apply -f namespace.json

## Minischritt 2:
# vi role.yaml 

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: training
  name: role-training-pods
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

kubectl apply -f role.yaml

## Minischritt 3:
## Verknüpfung Role zu Benutzer (=rolebinding) 

kubectl create rolebinding rolebindin-training-pods --role role-training-pods --user training

```



## Ref: 

 * https://docs.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengaddingserviceaccttoken.htm
 * https://microk8s.io/docs/multi-user
