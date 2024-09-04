# Complete Exercise working with Antrea - own cluster

## Our Goal 

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/14cc372e-e3df-4075-9330-d9ca50eab959)

## How the order of priorities work

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/8bf05e88-a7cd-4906-9271-eee2acc014a7)

## Our Setup 

```
In app1 are some Ubuntu Servers for Testing: dev-app1 / preprod-app1

1x Ubuntu Server 16.04
2x Ubuntu Server 20.04
In app2 is a simple 3 Tier-App (WEB-APP-DB): dev-app2 / preprod-app2 (3tier-app)

1x nginx TCP/80 (NodePort)
1x php TCP/80 (ClusterIP)
1x mysql TCP/3306 (ClusterIP)
```

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/067f275e-152c-42b6-82fd-7bebc6921cbb)

## Step 1: Rollout the pods (dev-app1/dev-app2)

```
cd
mkdir -p manifests
cd manifests
mkdir 10-antrea
cd 10-antrea
nano 01-pods-dev-app1-app2.yaml 
```

```
apiVersion: v1
kind: Namespace
metadata:
  name: dev-app1
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ubuntu-16-04
  labels:
    app: ubuntu-16-04
  namespace: dev-app1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ubuntu-16-04
  template:
    metadata:
      labels:
        app: ubuntu-16-04
    spec:
      containers:
      - name: ubuntu-16-04
        image: ubuntu:16.04
        imagePullPolicy: IfNotPresent
        command: [ "/bin/bash", "-c" ]
        args:
          - apt-get update;
            apt-get install iputils-ping -y;
            apt-get install net-tools;
            apt-get install curl -y;
            sleep infinity;
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ubuntu-20-04
  labels:
    app: ubuntu-20-04
  namespace: dev-app1
spec:
  replicas: 2
  selector:
    matchLabels:
      app: ubuntu-20-04
  template:
    metadata:
      labels:
        app: ubuntu-20-04
    spec:
      containers:
      - name: ubuntu-20-04
        image: ubuntu:20.04
        imagePullPolicy: IfNotPresent
        command: [ "/bin/bash", "-c" ]
        args:
          - apt-get update;
            apt-get install tcpdump -y;
            apt-get install telnet -y;
            apt-get install iputils-ping -y;
            apt-get install nmap -y;
            apt-get install net-tools;
            apt-get install netdiscover -y;
            apt-get install mysql-client -y;
            apt-get install curl -y;
            apt-get install dsniff -y;
            sleep infinity;
---
apiVersion: v1
kind: Namespace
metadata:
  name: dev-app2
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: default-conf
  namespace: dev-app2
data:
  default.conf: |
    server {
    listen 80 default_server;

    location / {
      proxy_pass http://app-service;
      proxy_http_version 1.1;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    }
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: dev-app2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
        service: web
        kind: dev
        type: internal
    spec:
      containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
        volumeMounts:
        - mountPath: /etc/nginx/conf.d # mount nginx-conf volumn to /etc/nginx
          readOnly: true
          name: default-conf
        - mountPath: /var/log/nginx
          name: log
      volumes:
      - name: default-conf
        configMap:
          name: default-conf # place ConfigMap `nginx-conf` on /etc/nginx
          items:
            - key: default.conf
              path: default.conf
      - name: log
        emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: dev-app2
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: appserver
  labels:
    app: app
  namespace: dev-app2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: app
        kind: dev
        type: internal
    spec:
      containers:
      - name: php-apache
        image: derstich/miserver:006
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: app-service
  labels:
    app: app
  namespace: dev-app2
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: app
---
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: mysql
  namespace: dev-app2
spec:
  selector:
    matchLabels:
      app: mysql8
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql8
        service: db
        kind: dev
        type: internal
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        imagePullPolicy: IfNotPresent
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: .sweetpwd.
        - name: MYSQL_DATABASE
          value: my_db
        - name: MYSQL_USER
          value: db_user
        - name: MYSQL_PASSWORD
          value: .mypwd
        args: ["--default-authentication-plugin=mysql_native_password"]
        ports:
        - containerPort: 3306
          name: mysql8
---
apiVersion: v1
kind: Service
metadata:
  name: mysql8-service
  labels:
    app: mysql8
  namespace: dev-app2
spec:
  type: ClusterIP
  ports:
  - port: 3306
    protocol: TCP
  selector:
    app: mysql8
```

```
kubectl apply -f .
kubectl -n dev-app1 get all
kubectl -n dev-app2 get all 
```

## Schritt 2: rollout preprod-app1/preprod-app2 

```
nano 02-deployment-preprod-app1-app2.yaml
```

```
apiVersion: v1
kind: Namespace
metadata:
  name: preprod
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ubuntu-16-04
  labels:
    app: ubuntu-16-04
  namespace: preprod-app1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ubuntu-16-04
  template:
    metadata:
      labels:
        app: ubuntu-16-04
    spec:
      containers:
      - name: ubuntu-16-04
        image: ubuntu:16.04
        imagePullPolicy: IfNotPresent
        command: [ "/bin/bash", "-c" ]
        args:
          - apt-get update;
            apt-get install iputils-ping -y;
            apt-get install net-tools;
            apt-get install curl -y;
            sleep infinity;
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ubuntu-20-04
  labels:
    app: ubuntu-20-04
  namespace: preprod-app1
spec:
  replicas: 2
  selector:
    matchLabels:
      app: ubuntu-20-04
  template:
    metadata:
      labels:
        app: ubuntu-20-04
    spec:
      containers:
      - name: ubuntu-20-04
        image: ubuntu:20.04
        imagePullPolicy: IfNotPresent
        command: [ "/bin/bash", "-c" ]
        args:
          - apt-get update;
            apt-get install tcpdump -y;
            apt-get install telnet -y;
            apt-get install iputils-ping -y;
            apt-get install nmap -y;
            apt-get install net-tools;
            apt-get install netdiscover -y;
            apt-get install mysql-client -y;
            apt-get install curl -y;
            apt-get install dsniff -y;
            sleep infinity;
---
apiVersion: v1
kind: Namespace
metadata:
  name: preprod-app2
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: default-conf
  namespace: preprod-app2
data:
  default.conf: |
    server {
    listen 80 default_server;

    location / {
      proxy_pass http://app-service;
      proxy_http_version 1.1;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    }
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: preprod-app2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
        service: web
        kind: dev
        type: internal
    spec:
      containers:
      - name: nginx
        image: nginx
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
        volumeMounts:
        - mountPath: /etc/nginx/conf.d # mount nginx-conf volumn to /etc/nginx
          readOnly: true
          name: default-conf
        - mountPath: /var/log/nginx
          name: log
      volumes:
      - name: default-conf
        configMap:
          name: default-conf # place ConfigMap `nginx-conf` on /etc/nginx
          items:
            - key: default.conf
              path: default.conf
      - name: log
        emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: preprod-app2
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: appserver
  labels:
    app: app
  namespace: preprod-app2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: app
        kind: dev
        type: internal
    spec:
      containers:
      - name: php-apache
        image: derstich/miserver:005
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: app-service
  labels:
    app: app
  namespace: preprod-app2
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: app
---
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: mysql
  namespace: preprod-app2
spec:
  selector:
    matchLabels:
      app: mysql8
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql8
        service: db
        kind: dev
        type: internal
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        imagePullPolicy: IfNotPresent
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: .sweetpwd.
        - name: MYSQL_DATABASE
          value: my_db
        - name: MYSQL_USER
          value: db_user
        - name: MYSQL_PASSWORD
          value: .mypwd
        args: ["--default-authentication-plugin=mysql_native_password"]
        ports:
        - containerPort: 3306
          name: mysql8
---
apiVersion: v1
kind: Service
metadata:
  name: mysql8-service
  labels:
    app: mysql8
  namespace: preprod-app2
spec:
  type: ClusterIP
  ports:
  - port: 3306
    protocol: TCP
  selector:
    app: mysql8

```

```
kubectl apply -f .
```

## Schritt 3: Daten auslesen 

```

# dev-app1
kubectl -n dev-app1 get pods -o=custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,STATUS:.status.phase,IP:.status.podIP,NODE:.spec.nodeName

# dev-app2 
kubectl -n dev-app2 get pods -o=custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,STATUS:.status.phase,IP:.status.podIP,NODE:.spec.nodeName

# preprod-app1
kubectl -n preprod-app1 get pods -o=custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,STATUS:.status.phase,IP:.status.podIP,NODE:.spec.nodeName

# preprod-app2 
kubectl -n preprod-app2 get pods -o=custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,STATUS:.status.phase,IP:.status.podIP,NODE:.spec.nodeName
```

```
# BITTE die Infos zwischen speichern oder Screenshot machen
```

## Schritt 4: Zugriffe auf dev-app2/prepod-app2 klären 

```
kubectl get svc -n dev-app2 nginx 
```

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/00a9d952-732a-4e12-98d5-766734e96ba7)

```
curl -i http://10.135.0.5:32767
# oder im Browser mit Public - IP
```

```
kubectl get svc -n preprod-app2 nginx 
```

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/0f53b7a4-0fe2-4294-b3eb-f5ac968da47c)

```
curl -i http://10.135.0.5:31836 
```

## Schritt 5: Zugriff ohne antrea policy testen 

```
KURZ=jm
kubectl exec -it -n dev-app1-$KURZ deployment/ubuntu-20-04 -- /bin/bash
```

```
# scannen des netzes
# !!! Achtung Netz kann anders sein !!!
nmap 10.244.0.0/22
```

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/33ee8c86-d3d9-46c6-9b67-93b3318a2739)

 * Namen werden aufgelöst (rückwärtig) 
 * alle ports sind einsehbar
 * Verbindung funktioniert nach überall

```
# mysql preprod herausfinden
# !!! Achtung Netz ändern 
nmap 10.244.0.0/22 | grep mysql | grep preprod
```

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/852f4781-fb45-45f6-9c8b-22474be6e092)

```
# Oh, wir haben das Passwort herausgefunden (Social Engineering ;o)) 
.sweetpwd. 
```

```
mysql -h 10-244-3-20.mysql8-service.preprod-app2.svc.cluster.local -p 
```

## Schritt 6: Isolieren von dev und preprod 

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/52d514ce-806b-48ed-bb90-8de5897537ef)

```
# Namspaces labeln
kubectl label ns dev-app1 env=dev ns=dev-app1
kubectl label ns dev-app2 env=dev ns=dev-app2
kubectl label ns preprod-app1 env=preprod ns=preprod-app1
kubectl label ns preprod-app2 env=preprod ns=preprod-app2

kubectl describe ns dev-app1
```

```
# now create the policy
# nano 10-deny-dev-to-preprod.yaml 
apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: deny-dev-to-preprod
spec:
    priority: 100
    tier: SecurityOps
    appliedTo:
      - namespaceSelector:
          matchLabels:
            env: preprod
    ingress:
      - action: Drop
        from:
          - namespaceSelector:
              matchLabels:
                env: dev
```

```
# Test ob ping von preprod nach dev funktioniert
# Hier ein POD-IP raussuchen 
kubectl -n dev-app1 get pods -o wide
kubectl -n preprod-app1 exec deployments/ubuntu-20-04 -- ping 10.244.3.15

# Test ob ping von dev nach preprod funktioniert - der sollte nicht funktionieren 
# Hier eine POD-IP rausschen 
kubectl -n preprod-app1 get pods -o wide
kubectl -n dev-app1 exec deployments/ubuntu-20-04 -- ping 10.244.2.25
```

```
# ClusterNetworkPolicy anwenden 
kubectl apply -f .
```

```
# Jetzt nochmal die Pings testen von oben
# ---> Ping ist immer noch möglich --> da keine Firewall - Regel 
kubectl -n preprod-app1 exec deployments/ubuntu-20-04 -- ping 10.244.3.15

# in die andere Richtung geht es aber nicht !!
kubectl -n dev-app1 exec deployments/ubuntu-20-04 -- ping 10.244.2.25
```

```
# ok jetzt in die andere richtung
# nano 15-deny-preprod-to-dev.yaml 
apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: deny-preprod-to-dev-<kurz-name>
spec:
    priority: 101
    tier: SecurityOps
    appliedTo:
      - namespaceSelector:
          matchLabels:
            env: dev-<name-kurz>
    ingress:
      - action: Drop
        from:
          - namespaceSelector:
              matchLabels:
                env: preprod-<name-kurz>
```

```
kubectl apply -f .
kubectl get clusternetworkpolicies
```

```
# Only output 
NAME                     TIER          PRIORITY   DESIRED NODES   CURRENT NODES   AGE
deny-dev-to-preprod-jm   SecurityOps   100        2               2               16m
deny-preprod-to-dev      SecurityOps   101        2               2               3m15s
```

```
# und jetzt geht pingen in die andere Richtung auch nicht mehr
kubectl -n preprod-app1-$KURZ exec deployments/ubuntu-20-04 -- ping 10.244.3.15
```

## Schritt 11: Isolate Pods (only within the namespaces) 

  * Aktuell ist das ping vom preprod-app1 zum preprod-app2 namespace noch möglich
  * Das wollen wir einschränken
  * Ausserdem von dev-app1 zu dev-app2 

```
# So sehen unsere Namespace - Labels aus
kubectl describe namespace dev-app1 
```

```
# Ausgabe, z.B. 
Name:         dev-app1-jm
Labels:       env=dev-jm
              ns=dev-app1-jm
```

```
# nano 20-allow-ns-dev-app1-dev-app1.yaml
# Traffic innerhalb des Namespaces erlaubt 

apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: 20-allow-ns-dev-app1-dev-app1
spec:
    priority: 100
    tier: application
    appliedTo:
      - namespaceSelector:
          matchLabels:
            ns: dev-app1
    ingress:
      - action: Allow
        from:
          - namespaceSelector:
              matchLabels:
                ns: dev-app1-<name-kurz>
```

```
kubectl apply -f .
```

```
# nano 25-drop-any-ns-dev-app2.yaml 
# allen anderen Traffic zum namespace app2 hin verbieten aus anderen namespaces 
apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: 25-drop-any-ns-dev-app2
spec:
    priority: 110
    tier: application
    appliedTo:
      - namespaceSelector:
          matchLabels:
            ns: dev-app2
    ingress:
      - action: Drop
        from:
          - namespaceSelector: {}
```

```
kubectl apply -f .
```

```
# nano 30-allow-ns-preprod-app1-preprod-app1.yaml 
# Same for preprod-app1
# Allow all traffic within namespace 
apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: 30-allow-ns-preprod-app1-preprod-app1
spec:
    priority: 120
    tier: application
    appliedTo:
      - namespaceSelector:
          matchLabels:
            ns: preprod-app1
    ingress:
      - action: Allow
        from:
          - namespaceSelector:
              matchLabels:
                ns: preprod-app1
```

```
kubectl apply -f .
```

```
# disallow all traffic from other namespaces to prepr
# nano 35-drop-any-ns-preprod-app2.yaml  
apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: 21-drop-any-ns-preprod-app2
spec:
    priority: 130
    tier: application
    appliedTo:
      - namespaceSelector:
          matchLabels:
            ns: preprod-app2-<name-kurz>
    ingress:
      - action: Drop
        from:
          - namespaceSelector: {}
```

```
kubectl apply -f .
```

## Schritt 12: Isolate traffic within app2 - namespaces (3-Tier-app)
```
# For dev-app2 we want
web->app (80)
app->db (3306) 
drop everything else 
```

```
kubectl -n dev-app2 describe pods | head -n 20
kubectl -n preprod-app2 describe pods | head -n 20
```

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/bfd0b89b-aa47-4493-8952-3c2aff5f7f1c)

  * we are using the label app=xxx

```
# nano 40-allow-web-app.yaml 
apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: 40-allow-web-app
spec:
    priority: 10
    tier: application
    appliedTo:
      - podSelector:
          matchLabels:
            app: app
    ingress:
      - action: Allow
        from:
          - podSelector:
              matchLabels:
                app: nginx
        ports:
          - protocol: TCP
            port: 80
```

```
kubectl apply -f  .
```

```
# nano 45-allow-app-db.yaml
apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: 02-allow-app-db
spec:
    priority: 20
    tier: application
    appliedTo:
      - podSelector:
          matchLabels:
            app: mysql8
    ingress:
      - action: Allow
        from:
          - podSelector:
              matchLabels:
                app: app
        ports:
          - protocol: TCP
            port: 3306
```

```
kubectl apply -f .
```

```
# nano 50-deny-any-to-app2.yaml
# Deny everything else
apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: 03-deny-any-to-app2
spec:
    priority: 30
    tier: application
    appliedTo:
      - namespaceSelector:
          matchLabels:
                  ns: dev-app2
      - namespaceSelector:
          matchLabels:
                  ns: preprod-app2
    ingress:
      - action: Drop
        from:
          - namespaceSelector: {}

```

```
kubectl apply -f .
```

## Schritt 13: Usage of the Emergency Tier - e.g. Attack 

  * We have problems with Ubuntu 16.04. an we want to isolate it.

```
kubectl get tiers
```

```
# nano 80-emergency.yaml 
apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: 50-deny-any-pod-ubuntu16
spec:
    priority: 50
    tier: emergency
    appliedTo:
      - podSelector:
          matchLabels:
                  app: ubuntu-16-04
    ingress:
      - action: Drop
        from:
          - namespaceSelector: {}
```

```
kubectl apply -f .
```

  * Because Emergency has the highest priority, the policy in application (allow any in ns-app1) has no Impact anymore.


## Reference: 

  * https://www.vrealize.it/2020/09/28/securing-you-k8s-network-with-antrea-clusternetworkpolicy/

