# Eigenen Header Abfragen 

## Vorbereitung 

```
cd
mkdir -p manifests/headertest
cd manifests/headertest
```

## Ohne Routing 

```
nano foo.yaml
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: foo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: foo
  template:
    metadata:
      labels:
        app: foo
    spec:
      containers:
        - name: foo
          image: hashicorp/http-echo
          args:
            - "-text=Hello from foo"
            - "-listen=:5678"
---
apiVersion: v1
kind: Service
metadata:
  name: foo
spec:
  selector:
    app: foo
  ports:
    - port: 80
      targetPort: 5678
```

```
nano ingress.yaml
```

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: header-routing
spec:
  ingressClassName: nginx
  rules:
    - host: <hier-domain>.t3isp.de
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: foo
                port:
                  number: 80
```

```
kubectl apply -f .
kubectl describe ingress header-routing
```

``` 
curl http://<eure-subdomain>.t3isp.de
```



## Walkthrough (Teil 2)

```
# ändern des der helm installation
mkdir helm-values
cd helm-values
nano values.yaml
```

```
controller:
   allowSnippetAnnotations: true
   snippetSecurityPolicy:
      enabled: true
      allowed-snippet-annotations: 'server-snippet,configuration-snippet,rewrite-snippet'
```

```
cd ..
helm upgrade --install nginx-ingress ingress-nginx/ingress-nginx --namespace ingress --create-namespace --version 4.12.2 -f helm-values/values.yaml
```

```
nano bar.yaml
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bar
spec:
  replicas: 1
  selector:
    matchLabels:
      app: bar
  template:
    metadata:
      labels:
        app: bar
    spec:
      containers:
        - name: bar
          image: hashicorp/http-echo
          args:
            - "-text=Hello from bar"
            - "-listen=:5678"
---
apiVersion: v1
kind: Service
metadata:
  name: bar
spec:
  selector:
    app: bar
  ports:
    - port: 80
      targetPort: 5678
```


```
nano ingress.yaml
```

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: header-routing
  annotations:
    nginx.ingress.kubernetes.io/server-snippet: |
      set $target_service "foo";
      if ($http_x_service_type = "bar") {
        set $target_service "bar";
      }
      rewrite_by_lua_block {
        local target = ngx.var.target_service
        ngx.var.upstream = target
      }
      proxy_pass http://$upstream;
spec:
  ingressClassName: nginx
  rules:
    - host: <hier-domain>.t3isp.de
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: foo
                port:
                  number: 80
```

```
kubectl apply -f . 
```

## Testen mit curl 

```
# Aufruf ohne header
# Achtung IP - anpassen (ip von ingress - controller svc) 
curl http://<hier-domain>.t3isp.de
```
# Aufruf mit header
curl -H "X-Service-Type: bar" <hier-domain>.t3isp.de
```
