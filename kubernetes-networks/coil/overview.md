# Coil 

## Opt-In egress-gateway (NAT-Service)

```
apiVersion: coil.cybozu.com/v2
kind: Egress
metadata:
  namespace: internet-egress
  name: nat
spec:
  replicas: 2
  destinations:
  - 0.0.0.0/0
  - ::/0
```

  * Not all Pods become the client of Egress. To become a client, Pods need to have special annotations like this:

```
apiVersion: v1
kind: Pod
metadata:
  namespace: default
  name: nat-client
  annotations:
    egress.coil.cybozu.com/internet-egress: nat
spec:
```

## Reference  

  * Refs: https://blog.kintone.io/entry/coilv2
  * https://github.com/cybozu-go/coil

