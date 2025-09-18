# Exercise networkpolicy calico

## Step 1:

```
cd
mkdir -p manifests
cd manifests
mkdir calico
cd calico
```

```
nano 01-gnp.yml
```

## Step 2: Set global policy

```
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: default-deny
spec:
  # Auf alle Namespaces außer kube-system und calico-system anwenden
  namespaceSelector: kubernetes.io/metadata.name not in {"kube-system","calico-system"}

  types:
  - Ingress
  - Egress

  # Egress-Ausnahmen (z. B. DNS)
  egress:
  - action: Allow
    protocol: UDP
    destination:
      selector: 'k8s-app == "kube-dns"'
      ports: [53]
  - action: Allow
    protocol: TCP
    destination:
      selector: 'k8s-app == "kube-dns"'
      ports: [53]
```

```
kubectl apply -f .
```

## Step 3: nginx ausrollen aus manifests/04-service und testen

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

## Step 4: Traffic erlauben egress von busybox 

```
nano 02-egress-allow-busybox.yml  
```

```
# vi 02-egress-allow-busybox.yml
apiVersion: projectcalico.org/v3
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

## Step 5: Traffic erlauben für nginx 

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
