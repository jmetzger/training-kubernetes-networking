# Install istio with helm 

## Walkthrough (Base) - Step 1 

```
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update

helm install istio-base istio/base -n istio-system --create-namespace 
helm ls -n istio-system

# Now we need to install the discovery service 
helm install istiod istio/istiod -n istio-system --wait
helm ls -n istio-system
# deployed ? 
helm status istiod -n istio-system

# Now find out if things were deployed properly 
kubectl get deployments -n istio-system --output wide

```

## Walkthrough (Gateway) - Step 2

```
# Install gateway 
helm install istio-ingress istio/gateway -n istio-ingress --create-namespace --wait
```


## Reference 

  * https://istio.io/latest/docs/setup/install/helm/
