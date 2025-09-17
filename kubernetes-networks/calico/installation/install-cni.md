# Install Calico - CNI 

## Walkthrough 

```
# Step 1 - Install the operator
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.3/manifests/tigera-operator.yaml
# Step 2 - Install the custom resources 
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.3/manifests/custom-resources.yaml
```

## Testing 

```
kubectl -n tigera-operator get pods 
kubectl -n calico-system get pods
kubectl -n calico-apiserver get pods
# Sind die nodes schon ready 
kubectl get nodes

kubectl -n kube-system get pods
kubectl -n kube-system get pods coredns-7c65d6cfc9-f6f56 -o wide
kubectl -n kube-system describe pods coredns-7c65d6cfc9-f6f56
```

## Reference 

  * https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart
