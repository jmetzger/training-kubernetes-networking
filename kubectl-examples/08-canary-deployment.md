# Exercise: Canary-Deployment 

## Phase 1: stable application (without canary)  

```
cd
cd manifests
mkdir ab 
cd ab 
```

```
# vi 01-cm-version1.yml 
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-version-1
data:
  index.html: |
    <html>
    <h1>Welcome to Version 1</h1>
    </br>
    <h1>Hi! This is a configmap Index file Version 1 </h1>
    </html>
```

```
# vi 02-deployment-v1.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy-v1
spec:
  selector:
    matchLabels:
      version: v1
  replicas: 20
  template:
    metadata:
      labels:
        app: nginx
        version: v1
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
            - name: nginx-index-file
              mountPath: /usr/share/nginx/html/
      volumes:
      - name: nginx-index-file
        configMap:
          name: nginx-version-1
```

```
# vi 05-svc.yml 
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    svc: nginx
spec:
  type: ClusterIP
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: nginx
```

```
kubectl apply -f .
kubectl run -it --rm podtest --image=busybox 
```

```
# wget -O - http://my-nginx.default.svc.cluster.local  
while [ true ]; do wget -O - http://my-nginx.default.svc.cluster.local; done  
```


## Step 2: Additional Deployment on top (only 2 vs. 20) 

```
nano 03-cm-version2.yml
```

```
# vi 03-cm-version2.yml 
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-version-2
data:
  index.html: |
    <html>
    <h1>Welcome to Version 2</h1>
    </br>
    <h1>Hi! This is a configmap Index file Version 2 </h1>
    </html>
```

```
nano 04-deployment-v2.yml
```

```
# vi 04-deployment-v2.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy-v2
spec:
  selector:
    matchLabels:
      version: v2
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
        version: v2
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
            - name: nginx-index-file
              mountPath: /usr/share/nginx/html/
      volumes:
      - name: nginx-index-file
        configMap:
          name: nginx-version-2
```

```
kubectl apply -f .
kubectl run -it --rm podtest --image=busybox 
```

```
# wget -O - http://my-nginx.default.svc.cluster.local  
while [ true ]; do wget -O - http://my-nginx.default.svc.cluster.local; done  
```

