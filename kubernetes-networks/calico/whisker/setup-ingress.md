# Setup Ingress for whisker 

## Important (current state: calico 3.30.x)

  * In current version by default traffic to whisker is not allowed (only through port-forwarding)
  * Whisker and Goldmane are the first components in calico that create NetworkPolicy Objects
  * Example:

![image](https://github.com/user-attachments/assets/9ae9e0bb-9039-4ade-9a6d-640e22b55333)

## Labelling of namespaces 

  * Namespaces are labelled when create

```
kubectl describe ns ingress
```

## Labels of pods (ingress controller)

```
kubectl -n ingress get pods --show-labels
```

## Prerequisites: 

  * ingress controller installed (nginx) in namespace ingress.
  * wildcard dns - setup for ip of ingress controller

```
# Look for external ip 
kubectl -n ingress get svc 
```

## Walkthrough 

```
cd
mkdir -p manifests/whisker
cd manifests/whisker
```

```
# credentials erstellen
htpasswd -c auth admin  # Enter your desired password
kubectl create secret generic whisker-basic-auth --from-file=auth -n calico-system
```

```
nano 01-ingress.yaml
```

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: whisker
  namespace: calico-system
  nginx.ingress.kubernetes.io/auth-type: basic
  nginx.ingress.kubernetes.io/auth-secret: whisker-basic-auth
  nginx.ingress.kubernetes.io/auth-realm: "Authentication Required"
spec:
  ingressClassName: nginx
  rules:
    - host: <hier-domain>.t3isp.de
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: whisker
                port:
                  number: 8081
```


```
nano 02-networkpolicy.yaml
```

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: whisker-ingress
  namespace: calico-system
spec:
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: ingress
      podSelector:
        matchLabels:
          app.kubernetes.io/component: controller
          app.kubernetes.io/name: ingress-nginx
    ports:
    - port: 8081
      protocol: TCP
  podSelector:
    matchLabels:
      app.kubernetes.io/name: whisker
  policyTypes:
  - Ingress
```

```
kubectl apply -f .
```

## Reference:

  * https://github.com/orgs/projectcalico/discussions/10395


