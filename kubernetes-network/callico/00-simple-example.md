# Simple Example Calico 

```
kubectl create ns policy-demo
kubectl create deployment --namespace=policy-demo nginx --image=nginx
kubectl expose --namespace=policy-demo deployment nginx --port=80
# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo access --rm -ti --image busybox /bin/sh
# innerhalb der shell 
wget -q nginx -O -




```




## Ref:

  * https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-basic
