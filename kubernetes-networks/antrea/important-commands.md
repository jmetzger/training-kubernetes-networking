# Important commands: antrea 

## Finding the bridge 

  * ovs-vsctl connects to an ovsdb-server process that maintains an Open vSwitch configuration database

```
# How to see the bridge
kubectl -n kube-system exec -it antrea-agent-79bx2 -c antrea-agent -- ovs-vsctl show

# or: always shows the first pod it finds
kubectl -n kube-system exec -it daemonset/antrea-agent -c antrea-agent -- ovs-vsctl show
```

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/d369db27-630a-4b9f-9d9b-834075514737)

## Show the configuraton settings of antrea 

```


```
