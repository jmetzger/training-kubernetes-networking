# Multiple nginx-Controller 

## Set different helm-values and install second release 

```
controller:
  electionID: ingress-controller-leader
  ingressClass: internal-nginx  # default: nginx
  ingressClassResource:
    name: internal-nginx  # default: nginx
    enabled: true
    default: false
    controllerValue: "k8s.io/internal-ingress-nginx"  # default: k8s.io/ingress-nginx
```

## Reference:

   * https://kubernetes.github.io/ingress-nginx/user-guide/multiple-ingress/
