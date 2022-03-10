# Assigning pods to nodes 

## Walkthrough 

```
# leave n3 as is 
kubectl label nodes n2 disktype=ssd
kubectl label nodes n1 disktype=ssd

# Let's rewrite that to deployment 
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disktype: ssd



```

## Ref:

  * https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
