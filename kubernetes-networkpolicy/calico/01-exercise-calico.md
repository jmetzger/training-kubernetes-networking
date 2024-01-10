# Exercise networkpolicy calico

## Step 1: Set global policy (Trainer only) 

```
apiVersion: crd.projectcalico.org/v1
kind: GlobalNetworkPolicy
metadata:
  name: default-deny
spec:
  namespaceSelector: kubernetes.io/metadata.name != "kube-system"
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
kubectl run -it --rm access --image=busybox 
```

```
# In der Bbusybox 
wget -O - http://my-nginx 
```

## Step 3: Traffic erlauben egress von busybox 

```
cd
cd manifests
mkdir cnp
cd cnp
```


```
# vi 02-egress-allow-busybox.yml
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

## Step 4: Traffic erlauben für nginx 

```
# 03-allow-ingress-my-nginx.yml 
apiVersion: crd.projectcalico.org/v1
kind: NetworkPolicy
metadata:
  name: allow-nginx-ingress
spec:
  selector: run == 'my-nginx'
  types:
  - Ingress
  ingress:
  - action: Allow
    source:
      selector: run == 'access'
```

```
kubectl apply -f .
```

```
kubectl run -it --rm access --image=busybox 
```

```
# In der Bbusybox 
wget -O - http://my-nginx 
```
