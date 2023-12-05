# How to find out about the different cidrs in the cluster ? 

## Cluster CIDR = Pods Network CIDR 

### Cilium 

```
kubectl -n kube-system describe deployments/cilium-operator | grep cluster-pool
```
## Pods - CIDR (per node) 

  * A subnet will be taken from the Pod Network Cidr (configure in the kube controller manager) 

### Cilium 

```
kubectl describe ciliumnodes | grep -A1 -i "pod cid rs"
```

## Service - CIDR

  * A bit hacky (send an invalid data within the service resource and parse the output), but actually working
  * Kubernetes Api does currently not provide an endpoint to show this  

```
SVCRANGE=$(echo '{"apiVersion":"v1","kind":"Service","metadata":{"name":"tst"},"spec":{"clusterIP":"1.1.1.1","ports":[{"port":443}]}}' | kubectl apply -f - 2>&1 | sed 's/.*valid IPs is //')
echo $SVCRANGE
```
`
```
# output e.g.
10.245.0.0/16
```
