# Pod 2 Pod Internals (Beispiel auf worker3 (node))

![images](/images/Calico_Same_Node_Flow.png)

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/ba9d497d-36ed-467f-9965-faad76a201cd)


## Übung:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-test
spec:
  nodeName: worker3
  containers:
    - name: nginx
      image: nginx:1.24
```

```
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  nodeName: worker3
  containers:
    - name: busybox
      image: busybox:1.35
      command: ["sleep", "3600"]
```

