# antctl 

## Install (externally as tool (not in pod)): uses .kube/config (Done by trainer) 

```
# as root
cd /usr/local/sbin  
curl -Lo ./antctl "https://github.com/antrea-io/antrea/releases/download/v1.13.2/antctl-$(uname)-x86_64"
chmod +x ./antctl
```

```
# run as unprivileged user having a .kube/config in homedir 
antctl version
```

## Shows feature-gates for controller and agent (using antctl client externally)

   * Shows both (for controller and for agent), when you do it externally as client-tool from outside pod 

```
antctl get featuregates
```

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/285069b5-5d6f-4b40-b828-24f2b8879e16)

## Use antctl from within agent 

```
kubectl -n kube-system exec -it ANTREA-AGENT_POD_NAME -n kube-system -c antrea-agent -- bash
```
  * or
```
kubectl -n kube-system exec -it daemonset/antrea-agent -n kube-system -c antrea-agent -- bash
```

```
antctl help
antctl log-level
antctl get featuregates
```
