# Connect from remote 

```
# On master-server get config 
microk8s config > remote_config 

# Download (scp config file) and store in .kube - folder  
cd ~
mkdir .kube
cd .kube 
scp master_server:/path/to/remote_config config 

# now you can execute all kubectl commands, but they are executed against remote server 
microk8s kubectl get pods 
# or if using kubectl or alias 
kubectl get pods 

# if you want to use a different kube config file, you can do like so 
kubectl --kubeconfig /home/myuser/.kube/myconfig

```
