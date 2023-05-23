# Calico 

## calicoctl Installation walkthrough 

### Version rausbekommen 

```
# welche version von calico setzen wir aktuell auf dem server ein 
kubectl -n kube-system get ds calico-node -o=jsonpath='{.spec.template.spec.containers[0].image}'
# docker.io/calico/node:v3.23.5

```
