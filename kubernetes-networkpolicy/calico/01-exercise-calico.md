# Exercise networkpolicy callico

## Step 1: Set global policy (Trainer only) 

```
apiVersion: crd.projectcalico.org/v1
kind: GlobalNetworkPolicy
metadata:
  name: default-deny
spec:
  selector: kubernetes.io/metadata.name not in  {'kube-system', 'calico-system', 'calico-apiserver'}
  types:
  - Ingress
  - Egress

```

```
kubectl apply -f . 
```

## Step 2: nginx ausrollen aus manifests/04-service und testen

```
cd 
cd manifests 
cd 04-service 
kubectl apply -f 01-deploy.yml
kubectl apply -f 02-service.yml
```

```
kubectl run -it --rm podtest --image=busybox 
```

```
# In der Bbusybox 
wget -O - http://my-nginx 
```

## Step 3: Traffic erlauben egress von busybox 

```
apiVersion: crd.projectcalico.org/v1
kind: NetworkPolicy
metadata:
  name: allow-busybox-egress
spec:
  selector: run == 'access'
  types:
  - Egress
  egress:
  - action: Allow
```

```
kubectl apply -f .
```

```
kubectl run -it --rm access --image=busybox
```

```
# sollte gehen 
wget -O - http://www.google.de

# sollte nicht funktionieren
wget -O - http://my-nginx
```
