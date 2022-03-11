# Kubernetes Policy Demo 

## Walkthrough 

```
cd
mkdir -p manifests/callico/example1 
cd manifests/callico/example1 
```

```
## Step 1: Create containers 

kubectl create -f https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/manifests/00-namespace.yaml
kubectl create -f https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/manifests/01-management-ui.yaml
kubectl create -f https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/manifests/02-backend.yaml
kubectl create -f https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/manifests/03-frontend.yaml
kubectl create -f https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/manifests/04-client.yaml

kubectl get pods --all-namespaces --watch
kubectl get ns 
```

```
## Step 2: Check connections in the browser (ui) 
## Use IP of one of your nodes here 
http://164.92.255.234:30002/
```

```
## Step 3: Download default-deny rules 
wget https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/policies/default-deny.yaml
## Let us have look into it 
## Deny all pods 
cat default-deny.yaml 
## Apply this for 2 namespaces created in Step 1
kubectl -n client apply -f default-deny.yaml 
kubectl -n stars apply -f default-deny.yaml 

```

```
## Step 4: Refresh UI and see, that there are no connections possilbe 
http://164.92.255.234:30002/
```

```
## Step 5: 
## Allow traffic by policy 
wget https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/policies/allow-ui.yaml
wget https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/policies/allow-ui-client.yaml
## Let us look into this: 
cat allow-ui.yaml 
cat allow-ui-client.yaml
kubectl apply -f allow-ui.yaml
kubectl apply -f allow-ui-client.yaml 

```

```
## Step 6: 
## Refresh management ui 
## Now all traffic is allowed
http://164.92.255.234:30002/
```

```
## Step 7:
## Restrict traffic to backend 
wget https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/policies/backend-policy.yaml
cat backend-policy.yaml 
kubectl apply -f backend-policy.yaml


```

```
## Step 8:
## Refresh 
# The frontend can now access the backend (on TCP port 6379 only).
# The backend cannot access the frontend at all.
# The client cannot access the frontend, nor can it access the backend
http://164.92.255.234:30002/


```

```
## Step 8: 
wget https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/policies/frontend-policy.yaml
cat frontend-policy.yaml 
kubectl apply -f frontend-policy.yaml 
```

```
## Step 8:
# Refresh ui 
# Client can now access Frontend 
http://164.92.255.234:30002/

```

```
# Alles wieder löschen 
kubectl delete ns client stars management-ui


```



## Reference 

  * https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/kubernetes-demo
