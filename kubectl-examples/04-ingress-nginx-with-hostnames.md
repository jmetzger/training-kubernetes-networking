# Ingress Nginx 

## Walkthrough 

### Step 1: pods and services

```
cd
mkdir -p manifests
cd manifests 
mkdir abi
cd abi
```

```
nano apple.yml
```

```
# apple.yml 
# vi apple.yml 
kind: Pod
apiVersion: v1
metadata:
  name: apple-app
  labels:
    app: apple
spec:
  containers:
    - name: apple-app
      image: hashicorp/http-echo
      args:
        - "-text=apple-<dein-name>"
---

kind: Service
apiVersion: v1
metadata:
  name: apple-service
spec:
  selector:
    app: apple
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5678 # Default port for image
```

```
kubectl apply -f apple.yml 
```

```
nano banana.yml
```


```
# banana
# vi banana.yml
kind: Pod
apiVersion: v1
metadata:
  name: banana-app
  labels:
    app: banana
spec:
  containers:
    - name: banana-app
      image: hashicorp/http-echo
      args:
        - "-text=banana-<dein-name>"

---

kind: Service
apiVersion: v1
metadata:
  name: banana-service
spec:
  selector:
    app: banana
  ports:
    - port: 80
      targetPort: 5678 # Default port for image
```

```
kubectl apply -f banana.yml
```

## Step 2: Ingress 

```
nano ingress.yaml
```

```
# Ingress
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: "<euername>.lab1.t3isp.de"
    http:
      paths:
        - path: /apple
          backend:
            serviceName: apple-service
            servicePort: 80
        - path: /banana
          backend:
            serviceName: banana-service
            servicePort: 80
```

```
# ingress 
kubectl apply -f ingress.yml
kubectl get ing 
```

## Reference 

  * https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html

## Find the problem 

```
# Hints 

# 1. Which resources does our version of kubectl support 
# Can we find Ingress as "Kind" here.
kubectl api-ressources 

# 2. Let's see, how the configuration works 
kubectl explain --api-version=networking.k8s.io/v1 ingress.spec.rules.http.paths.backend.service

# now we can adjust our config 
```

## Direct Solution

```
nano ingress.yaml
```

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
    # with the ingress controller from helm, you need to set an annotation 
    # old version useClassName instead 
    # otherwice it does not know, which controller to use
    # kubernetes.io/ingress.class: nginx 
spec:
  ingressClassName: nginx
  rules:
# <deinname> ersetzen durch dienen namen, z.B. jochen
# jochen.lab1.t3isp.de
  - host: "<deinname>.lab1.t3isp.de"
    http:
      paths:
        - path: /apple
          pathType: Prefix
          backend:
            service:
              name: apple-service
              port:
                number: 80
        - path: /banana
          pathType: Prefix
          backend:
            service:
              name: banana-service
              port:
                number: 80                
```

```
kubectl apply -f .
kubectl describe ingress example-ingress 
```

```
# Testen im browser oder curl mit hostnamen
# Variante 1:
# z.B.
curl http://jochen.lab1.t3isp.de/apple
curl http://jochen.lab1.t3isp.de/banana
```
