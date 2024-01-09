# Get the different CIDRs in Calico 

## Service-CIDR 

  * kubectl cluster-info dump | grep -m 1 service-cluster-ip-range

## Cluster-CIDIR 

  * kubectl cluster-info dump | grep -m1 cluster-cidr 

## Pod-CIDR (works for all CNI's) 

```
kubectl describe nodes | grep -B 25 -i podcidr
```
