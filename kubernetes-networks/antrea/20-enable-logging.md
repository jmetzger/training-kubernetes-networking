# Enable Logging

## Steps 

```
# Activate Logging in Policy Ingress or Egress

# nano 80-emergency.yaml
apiVersion: crd.antrea.io/v1beta1
kind: ClusterNetworkPolicy
metadata:
  name: 80-emergency-ubuntu16
spec:
    priority: 50
    tier: emergency
    appliedTo:
      - podSelector:
          matchLabels:
                  app: ubuntu-16-04
    ingress:
      - action: Drop
        enableLogging: true
        from:
          - namespaceSelector: {}

```

```
kubectl apply -f .
```

```
# On which node is it running ?
kubectl -n dev-app1 -l app=ubuntu-16 -o wide
# Ausgabe: worker1 
```

```
# Connect to worker1 per ssh
tail /var/log/antrea/networkpolicy/np.log 
```

