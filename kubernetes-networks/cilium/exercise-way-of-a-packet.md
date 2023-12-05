# Exercise way of a packet 

## Vorbereitung:

```
# Achtung: Nodes anpassen
NODE1=node/telekom-xwkb3
NODE2=node/telekom-xwkb8
```

```
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: curl
  name: curl
spec:
  containers:
  - image: curlimages/curl
    name: curl
    command: ["sleep", "365d"]
  nodeName: $NODE1
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: httpbin
  name: httpbin
spec:
  containers:
  - image: kennethreitz/httpbin
    name: httpbin
  nodeName: $NODE2
EOF

```
