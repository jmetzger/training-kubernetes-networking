# Hubble exercise CLI 

![Star Wars Demo](https://docs.cilium.io/en/stable/_images/cilium_http_gsg.png)

## Step 1: Install demo 

```
cd
cd manifests
mkdir sw
cd sw 
curl https://raw.githubusercontent.com/cilium/cilium/1.14.4/examples/minikube/http-sw-app.yaml > sw.yaml
```

```
# Should get installed into your namespace / different than on overview image  
kubectl apply -f .
```

## Step 2: calico fw- Regel

```
nano rules.yaml
```

```
apiVersion: "cilium.io/v2"
kind: CiliumNetworkPolicy
metadata:
  name: "rule1"
spec:
  description: "L3-L4 policy to restrict deathstar access to empire ships only"
  endpointSelector:
    matchLabels:
      org: empire
      class: deathstar
  ingress:
  - fromEndpoints:
    - matchLabels:
        org: empire
    toPorts:
    - ports:
      - port: "80"
        protocol: TCP
```

```
kubectl apply -f .
```


## Step 3: Produce some traffic 

```
# Let's produce some traffic
# Nehmt bitte hier Euren namespace / er mag ansonsten nicht auflösen
# Was mir noch ein Problem im cluster zu sein scheint.
kubectl exec tiefighter -- curl -s -XPOST deathstar.jochen.svc.cluster.local/v1/request-landing
```

## Reference 

  * https://docs.cilium.io/en/stable/gettingstarted/demo/#starwars-demo
