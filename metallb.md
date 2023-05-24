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
      # Take the single address in case of digitalocean here.
      # External ip 
      # - 192.168.1.240-192.168.1.250
      - 61.46.56.21
```

```
vi 02-svc.yml 
```

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc 
spec:
  selector:

# Adjust -> selector -> according to nginx below 
    app: nginx 
  ports:
  - name: http
    port: 80
    targetPort: 80
  type: LoadBalancer
  # uncomment to try, if you get it automatically 
  loadBalancerIP: 61.46.56.21

```

```
kubectl apply -f .
kubectl -n metallb-system get svc my-service 
```

```
kubectl create deployment nginx --image nginx:alpine --port 80 --replicas=1
kubectl get svc nginx-svc
# You can open 80 port on Firewall using Console and open http://167.99.99.99 for a test.
```

## Trafic Policy 

 * https://metallb.universe.tf/usage/
