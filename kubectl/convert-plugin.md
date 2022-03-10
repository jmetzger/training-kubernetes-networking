# kubectl convert plugin 

## What is about? 

  * Plugins needs to be installed seperately on Client (or where you have your manifests) 

## Walkthrough 

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert"
# Validate the checksum
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert.sha256"
echo "$(<kubectl-convert.sha256) kubectl-convert" | sha256sum --check
# install 
sudo install -o root -g root -m 0755 kubectl-convert /usr/local/bin/kubectl-convert

# Does it work 
kubectl convert --help 

# Works like so 
# Convert to the newest version 
# kubectl convert -f pod.yaml

```

## Reference 

  * https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-convert-plugin 
