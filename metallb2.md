# Metallb 

## Attention 

  * On digitalocean, we will probably run into problems, that it is not working properly 

## General 

  * Supports bgp and arp 
  * Divided into controller, speaker 

## Installation Ways  

  * helm 
  * manifests 

## Walkthrough Digitalocean 

```
# Just to show some basics 
# Page from metallb says that digitalocean is not really supported well 
# So we will not install the speaker .

helm repo add metallb https://metallb.github.io/metallb 
```

```
# Eventually disabling speaker 
# vi values.yml 

```

```
helm install metallb metallb/metallb --namespace=metallb-system --create-namespace 
```

```
cd
mkdir -p manifests
cd manifests 
mkdir mb 
cd mb 
vi 01-cm.yml 
```

```
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.1.240-192.168.1.250

```

```
vi 02-svc.yml 
```

```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
  - name: http
    port: 80
    targetPort: 80
  type: LoadBalancer
  loadBalancerIP: 192.168.1.245

```

```
kubectl apply -f .
kubectl -n metallb-system get svc my-service 
```
