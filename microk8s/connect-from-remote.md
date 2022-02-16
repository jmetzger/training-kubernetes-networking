# Connect from remote 

```
# on client install kubectl 
sudo snap install kubectl --classic 

# On master-server get config 
# als root
cd
microk8s config > remote_config 

# Download (scp config file) and store in .kube - folder  
cd ~
mkdir .kube
cd .kube  # Wichtig: config muss nachher im verzeichnis .kube liegen 
# scp master_server:/path/to/remote_config config 
# z.B. 
scp 192.168.56.102:/root/remote_config config

# now you can execute all kubectl commands, but they are executed against remote server 
microk8s kubectl get pods 
# or if using kubectl or alias 
kubectl get pods 

# if you want to use a different kube config file, you can do like so 
kubectl --kubeconfig /home/myuser/.kube/myconfig

```
