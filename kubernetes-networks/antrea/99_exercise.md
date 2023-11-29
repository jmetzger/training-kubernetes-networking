# Complete Exercise working with Antrea (Designed for groups) 

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

## Step 1: Rollout the pods (dev)

  * Important - you need to adjust the namespaces as follows:
    * dev-app1-<name-kurz> -> z.B. dev-app1-jjm (Deine Initialien)

```
cd
mkdir -p manifests
cd manifests
mkdir 10-antrea
cd 10-antrea
```

```
# nano 01-deployment-dev.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev-app1-<name-kurz>
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ubuntu-16-04
  labels:
    app: ubuntu-16-04
  namespace: dev-app1-<name-kurz>
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
  namespace: dev-app1-<name-kurz>
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
```

```
kubectl apply -f .
```

## Reference: 

  * https://www.vrealize.it/2020/09/28/securing-you-k8s-network-with-antrea-clusternetworkpolicy/

