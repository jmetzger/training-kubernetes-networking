# Architecture and components 

## Overview 

![Overview](https://antrea.io/docs/v1.3.0/docs/assets/arch.svg.png)

## Components 

### antrea-controller 

  * Watches kube-api-server for changes on
    * pod
    * namespaces
    * NetworkPolicy
   
### antrea-agent 

  * Runs on every pod, deployed by Daemonset ? 

## Reference 

  * https://antrea.io/docs/v1.3.0/docs/design/architecture/
