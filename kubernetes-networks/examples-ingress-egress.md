# Examples ingress / egress 

```


```

## Downside egress 

  * No valid api for anything other than 
  * If you want more, you have to use CNI-Plugin specific, e.g. 

## Example Advanced Egress (cni-plugin specific) 

### Cilium

```
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: "fqdn-pprof"
  namespace: msp
spec:
  endpointSelector:
    matchLabels:
      app: pprof
  egress:
  - toFQDNs:
    - matchPattern: '*.baidu.com'
  - toPorts:
    - ports:
      - port: "53"
        protocol: ANY
      rules:
        dns:
        - matchPattern: '*'
```
