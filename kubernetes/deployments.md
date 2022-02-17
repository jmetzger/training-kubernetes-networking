# Deployments 

## Hierarchy 

```
deployment 
  replicaset 
    pod 


Deployment :: create a new replicaset, when needed (e.g. new version of image comes out) 
Replicaset :: manage the state - take care, that the are always x-pods running (e.g. 3) 
Pod :: create the containers 

```


## What are deploments 

  * Help to manage updates of pods / replicaset (rolling update) 


## Example 

```
# Deploy a sample from k8s.io 
kubectl apply -f https://k8s.io/examples/controllers/nginx-deployment.yaml

```

## Refs:

  * https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
