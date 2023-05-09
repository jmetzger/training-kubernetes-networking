# Metallb - LoadBalancer 

## When ?

  * You have a cluster on premise and need a load balancer 

## Walkthrough 

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.9/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.9/manifests/metallb.yaml
```

```
# On first install only
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey=”$(openssl rand -base64 128)”
```

```
# Create ConfigMap, use Layer 2 Configuration:
# https://metallb.universe.tf/configuration/
# vi config.yaml
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
    - 167.99.99.99–167.99.99.99
# The trick is to use range of IP addresses with the one external IP address of the Node02 (Node01 is a Master), in my case: 167.99.99.99. It allows to have full access from the Internet to our resources in Kubernetes.
```

```
# do we really need that (netplan) ?
# vi /etc/netplan/50-cloud-init.yaml
# netplan apply
kubectl apply -f config.yaml
```

```
# If you need to correct IP address:

kubectl edit cm -n metallb-system config
Create a web server and test connection.
```
```
kubectl create deployment nginx --image nginx:alpine --port 80 --replicas=1
kubectl expose deployment nginx --name=nginx-svc --port=80 --type=LoadBalancer
kubectl get nginx-svc
# You can open 80 port on Firewall using Console and open http://167.99.99.99 for a test.

```
