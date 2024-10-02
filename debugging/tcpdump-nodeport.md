# tcpdump - nodeport example 

```
## Übung 3.1  tcpdump 

0. export KUBECONFIG=~/.kube/config.calico

1.  manifest/03-deploy 

02-service.yaml 
-> type: NodePort

deploy.yaml 
replicas: 1 

2. kubectl apply -f .

3.  NodePort rausfinden

kubectl get svc svc-nginx 
-> 32682 

worker1 -> 164.92.131.128

4. bashrc richtig setzen 

 echo "export KUBECONFIG=~/.kube/config.calico" >> ~/.bashrc

5. Zweite ssh-session 

kubectl cluster-info # -> passt das ?

# ein debug-pod auf worker1 starten 
kubectl debug -it node/worker1 --image nicolaka/netshoot 
.# im pod 
tcdump -i eth0 port 32682  

6. im browser addresse eingeben 
http://164.92.131.128:32682

7. in der 1. ssh session 

kubectl get pods 
kubectl debug -it nginx-deployment-5948f7484f-sbq9v --image nicolaka/netshoot 
```

