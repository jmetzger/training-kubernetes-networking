# Overview

## Basics

  * Created by vmware
  * Uses Open VShift (virtuell Switches)
  * Kernel-Modul openswitch.ko takes care of traffic (performant)

## Basic Commands with antctl 

  * You will find this within antrea-agent

```
# on kube-system
kubectl -n kube-system get üpods 
kubectl -n kube-system exec -it ANTREA-AGENT_POD_NAME -n kube-system -c antrea-agent bash

antctl help
antctl log-level
antctl get featuregates 
```





## Architecture 

  * https://antrea.io/docs/v1.3.0/docs/design/architecture/
