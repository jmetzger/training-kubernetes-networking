# Enable hubble 

## What is it ?

  * Observability platform built on cilium 

## What is the current status of our system 

```
# you need to install the cilium client 
cilium status
```

## How to activate 

```
cilium hubble enable --ui
```

## Client needed 

  * hubble client zu installieren
  * https://docs.cilium.io/en/stable/gettingstarted/hubble_setup/


## References 

  * https://docs.cilium.io/en/stable/gettingstarted/hubble_setup


![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/33d346c3-44ab-4274-9f3c-af36c2122a0a)




## Probleme (evtl.)

### Digitalocean 

Activate hubble (on digitalocean doks) 

```
# Enable hubble
cilium hubble enable
```

  * Result:

```
tln1@client-telekom:~$ cilium hubble enable

Error: Unable to enable Hubble: release: not found
```

## How to fix ?

```
# First of all, it seems not be a good idea to install hubble on
# kubernetes doks (Digitalocean Kubernetes)
# because we might end up with 0 functionality concerning the cluster
# So better use a fresh on-premise cluster

# THEN, we can install hubble with helm
# But looks like cilium should already have been installed with helm

```

```
helm upgrade cilium cilium/cilium --version 1.11.7 \
   --namespace kube-system \
   --reuse-values \
   --set hubble.relay.enabled=true \
   --set hubble.ui.enabled=true
```

### Reference for hubble 

  * https://blog.ediri.io/how-to-enable-the-cilium-hubble-ui-in-a-civo-k3s-cluster
