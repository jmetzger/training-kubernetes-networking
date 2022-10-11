# Installation on Ubuntu (snap)

## Walkthrough

```
sudo snap install microk8s --classic
microk8s status

# Sobald Kubernetes zur Verfügung steht aktivieren wir noch das plugin dns
microk8s enable dns 
microk8s status

# Execute kubectl commands like so 
microk8s kubectl
microk8s kubectl cluster-info

# Make it easier with an alias 
echo "alias kubectl='microk8s kubectl'" >> ~/.bashrc
source ~/.bashrc
kubectl

```
## Working with snaps 

```
snap info microk8s 

```

## Ref:

  * https://microk8s.io/docs/setting-snap-channel
