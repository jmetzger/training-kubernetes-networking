# Install Calico - CNI 

## Walkthrough 

```
# Step 1 - Install the operator
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.1/manifests/tigera-operator.yaml
# Step 2 - Install the custom resources 
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.1/manifests/custom-resources.yaml
```

## Reference 

  * https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart
