# Architecture and components 

## Overview 

![Overview](https://antrea.io/docs/v1.3.0/docs/assets/arch.svg.png)

## Basics

  * Created by vmware
  * Uses Open VShift (virtuell Switches)
  * Kernel-Modul openswitch.ko takes care of traffic (performant)

## Components 

### antrea-controller 

  * Watches kube-api-server for changes on
    * pod
    * namespaces
    * NetworkPolicy
  * Implementation of Controller - API-Server
  * Reachable over kube-api-server by implementation https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/apiserver-aggregation/
   
### antrea-agent 

  * Runs on every pod, deployed by Daemonset ?
  * has an endpoint running gRPC which the controller connects to

## antctl 

  * cli for some debugging
  * controller-mode on controller (accessing from within controller pod)
  * agent-mode on agent (accessing from within agent-pod)
  * external also possible - uses kubeconfig to connect
    * Connection is done through kube-api-server

### Important antctl commands 

  * antclt help 

## Reference 

  * https://antrea.io/docs/v1.3.0/docs/design/architecture/
