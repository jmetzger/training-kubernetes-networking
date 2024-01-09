# Get the different CIDRs in Calico 

## Service-CIDR 

  * kubectl cluster-info dump | grep -m 1 service-cluster-ip-range

## Cluster-CIDIR 

  * kubectl cluster-info dump | grep -m1 cluster-cidr 

## Pod-CIDR (works for all CNI's) 

```
kubectl get describe nodes | grep -B -i podcidr
```
