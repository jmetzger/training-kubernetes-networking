# Architecture and components 

## Overview 

![Overview](https://antrea.io/docs/v1.3.0/docs/assets/arch.svg.png)

## Basics

  * Created by vmware
  * Uses Open VShift (virtuell Switches)
  * Kernel-Modul openswitch.ko takes care of traffic (performant)

## Components 

### antrea-controller (+api)

  * Watches kube-api-server for changes on
    * pod
    * namespaces
    * NetworkPolicy
  * Implementation of Controller - API-Server
  * Reachable over kube-api-server by implementation https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/apiserver-aggregation/
  * Currently only 1 replica is supported
  * computes NetworkPolicies and distributes them to the Antrea agents 

#### antrea controller api - part (how authentication works) 

  * The Controller API server delegates authentication and authorization to the Kubernetes API
  * the Antrea Agent uses a Kubernetes ServiceAccount token to authenticate to the Controller,
  * the Controller API server validates the token and whether the ServiceAccount is authorized for the API request with the Kubernetes API.

### antrea-agent 

  * Runs on every pod, deployed by Daemonset 
  * has an endpoint running gRPC which the controller connects to
  * Agents connect to controller api by ClusterIP - wit a service Account
  * Authentication is done through the kubernetes api server 

## antctl 

  * cli for some debugging
  * controller-mode on controller (accessing from within controller pod)
  * agent-mode on agent (accessing from within agent-pod)
  * external also possible - uses kubeconfig to connect
    * Connection is done through kube-api-server

### Important antctl commands 

```
# on kube-system
kubectl -n kube-system get üpods 

antctl get featuregates 
```

## Reference 

  * https://antrea.io/docs/v1.3.0/docs/design/architecture/
