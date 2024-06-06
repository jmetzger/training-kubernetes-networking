# Exercise networkpolicy calico

## Step 1: Set global policy

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
  egress:
   # allow all namespaces to communicate to DNS pods
  - action: Allow
    protocol: UDP
    destination:
      selector: 'k8s-app == "kube-dns"'
      ports:
      - 53
  - action: Allow
    protocol: TCP
    destination:
      selector: 'k8s-app == "kube-dns"'
      ports:
      - 53

```

```
kubectl apply -f . 
```

## Step 2: nginx ausrollen aus manifests/04-service und testen

```
cd
mkdir -p manifests
cd manifests
mkdir 04-service 
cd 04-service 
```

```
nano deploy.yml 
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-nginx
spec:
  selector:
    matchLabels:
      web: my-nginx
  replicas: 2
  template:
    metadata:
      labels:
        web: my-nginx
    spec:
      containers:
      - name: cont-nginx
        image: nginx
        ports:
        - containerPort: 80
```

```
nano service.yml
```


```
apiVersion: v1
kind: Service
metadata:
  name: svc-nginx
  labels:
    run: svc-my-nginx
spec:
  type: ClusterIP
  ports:
  - port: 80
    protocol: TCP
  selector:
    web: my-nginx      
        
```        

```
kubectl apply -f . 
```

```
kubectl run -it --rm access --image=busybox 
```
```

# In der Bbusybox 
wget -O - http://svc-nginx 
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
