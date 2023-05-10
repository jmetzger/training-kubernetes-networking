# Install istio with helm + walkthrough

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

## What about setting injection for the namespace ?

```
# https://istio.io/latest/docs/setup/additional-setup/sidecar-injection/
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.17/samples/sleep/sleep.yaml

# only 1 container in "sleep" pod 
kubectl get pods 
```

```
# Enable workspace with injection 
kubectl label namespace default istio-injection=enabled --overwrite
kubectl get namespace -L istio-injection

```

```
# Delete pod and watch it being recreated 
kubectl delete po -l app=sleep 
kubectl get pods 
# now we should see pods with 2 containers 
# NAME                    READY   STATUS    RESTARTS   AGE
# sleep-bc9998558-zhqpz   2/2     Running   0          5s

```

## Step 4: Rollout bookinfo samples 

```
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.17/samples/bookinfo/platform/kube/bookinfo.yaml
kubectl get pods 
kubectl get svc 
```

## Step 5: Check, if app is running 

```
kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"
```

## Reference 

  * https://istio.io/latest/docs/setup/install/helm/
  * https://istio.io/latest/docs/setup/additional-setup/sidecar-injection/
  * https://istio.io/latest/docs/setup/getting-started/#bookinfo
  * https://istio.io/latest/docs/tasks/observability/kiali/
