# Get the different CIDRs in Calico 

## Service-CIDR 

  * kubectl cluster-info dump | grep -m 1 service-cluster-ip-range

## Cluster-CIDR 

  * kubectl cluster-info dump | grep -m1 cluster-cidr
  * or # look into calico ippool / default is setup on installation of cni (calico)
  * kubectl -n calico-system describe ippool default-ipv4-ippool


## Pod-CIDR (works for all CNI's) 

```
kubectl describe nodes | grep -B 25 -i podcidr
```
