# Metrics-Server (microk8s) aktivieren 

## Warum ? Was macht er ? 

```
Der Metrics-Server sammelt Informationen von den einzelnen Nodes und Pods
Er bietet mit 

kubectl top pods
kubectl top nodes 

ein einfaches Interface, um einen ersten Eindruck über die Auslastung zu bekommen. 
```

## Walktrough 

```
# Auf einem der Nodes im Cluster (HA-Cluster) 
microk8s enable metrics-server 

# Es dauert jetzt einen Moment bis dieser aktiv ist auch nach der Installation 
# Auf dem Client
kubectl top nodes 
kubectl top pods 

```

## Kubernetes 

  * https://kubernetes-sigs.github.io/metrics-server/
  * kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
