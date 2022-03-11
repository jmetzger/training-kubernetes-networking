# Create a service user for kubeconfig 

```
# Prerequisites: you must currently be able to use kubectl (access allowed) 

kubectl -n kube-system create serviceaccount alice
TOKENNAME=`kubectl -n kube-system get serviceaccount alice -o jsonpath='{.secrets[0].name}'`
TOKEN=`kubectl -n kube-system get secret $TOKENNAME -o jsonpath='{.data.token}'| base64 --decode`

# Now adjust your config to have another context: alice-context 
# After that: 
kubectl config use-context alice-context
kubectl get pods 
# you can currently not do anything, because you have no permissions 
Error from server (Forbidden): pods is forbidden: User "system:serviceaccount:kube-system:alice" cannot list resource "pods" in API group "" in the namespace "default"

```
