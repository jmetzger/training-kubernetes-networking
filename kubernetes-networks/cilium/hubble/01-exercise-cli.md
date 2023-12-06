# Hubble exercise CLI 

![Star Wars Demo](https://docs.cilium.io/en/stable/_images/cilium_http_gsg.png)

## Step 1: Install demo 

```
cd
cd manifests
mkdir sw
cd sw 
curl https://raw.githubusercontent.com/cilium/cilium/1.14.4/examples/minikube/http-sw-app.yaml
```

```
# Should get installed into your namespace / different than on overview image  
kubectl apply -f http-sw-app.yaml
```

## Step 2: Produce some traffic 

```
# Let's produce some traffic
# Nehmt bitte hier Euren namespace / er mag ansonsten nicht auflösen
# Was mir noch ein Problem im cluster zu sein scheint.
kubectl exec tiefighter -- curl -s -XPOST deathstar.jochen.svc.cluster.local/v1/request-landing
```

