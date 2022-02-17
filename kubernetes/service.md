# Service 

## Was ? 

```
Stellt eine Netzwerkverbindung zu verschiedenen Pods her,
auf Basis eines Labels 

```

## Warum ? 

```
service (-controller) überprüft welche Nodes mit entsprechenden 
Label zur Verfügung stehen und übernimmt das Routing 

standardmäßig: round robin 
```

## What are services ? 

  * Services help you to connect to the pods seemlessly 
  * Service knows which pods are available

## service - types 

```
The type defines how the connection is done (what kind of network/ip/port is provided
to connect to the service 

ClusterIP
NodePort 
LoadBalancer - an external balancer is used (that is mainly the case in 
```

## Reference:

  * https://kubernetes.io/docs/concepts/services-networking/service/
