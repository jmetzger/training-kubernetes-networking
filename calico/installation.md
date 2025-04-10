# Installation calico - cni (Quickstart)

## Step 1: CNI-Setup (calico) on controlnode (login through ssh) 

```
kubectl get nodes 
```

```
# Output
root@controlplane:~# kubectl get nodes
NAME           STATUS     ROLES           AGE     VERSION
controlplane   NotReady   control-plane   6m27s   v1.28.6
worker1        NotReady   <none>          3m18s   v1.28.6
worker2        NotReady   <none>          2m10s   v1.28.6
worker3        NotReady   <none>          60s     v1.28.6
```

```
# Installing calico CNI 
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/tigera-operator.yaml
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/custom-resources.yaml
kubectl get ns
kubectl -n calico-system get all
kubectl -n calico-system get pods -o wide -w 
```

```
# After if all pods are up and running -> CTRL + C
```

```
kubectl -n calico-system get pods -o wide
# all nodes should be ready now 
kubectl get nodes -o wide 
```

```
# Output
root@controlplane:~# kubectl get nodes
NAME           STATUS   ROLES           AGE    VERSION
controlplane   Ready    control-plane   14m    v1.28.6
worker1        Ready    <none>          11m    v1.28.6
worker2        Ready    <none>          10m    v1.28.6
worker3        Ready    <none>          9m9s   v1.28.6
```
