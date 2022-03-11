# RBAC - Create user

## Enable RBAC in microk8s 

```
microk8s enable rbac 

```

## systemaccount anlegen und in kubeconfig hinterlegen 

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

## Ref: 

 * https://docs.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengaddingserviceaccttoken.htm
 * https://microk8s.io/docs/multi-user
