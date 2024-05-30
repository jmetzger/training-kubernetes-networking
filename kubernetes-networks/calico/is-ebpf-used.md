# Find out, if ebpf is used .. (extended Berkeley Paket Filter) 

  * Hint: By default this should not be activated

## Version microk8s 

```
kubectl -n kube-system logs calico-node-78s8q | grep -i bpfenabled
```

## Version installed on your own in cluster, e.g. kubeadm 

```
# Is in different namespace in this case 
kubectl -n calico-system logs calico-node-78s8q | grep -i bpfenabled
```
