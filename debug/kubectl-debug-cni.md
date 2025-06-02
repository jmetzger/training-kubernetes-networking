## Debug node / Get access to filesystem (readonly to host)

```
kubectl debug node/worker1 -it --image=ubuntu 
# in der bash 
cd /host/etc/cni/net.d
ls -la
apt update 
apt install jq # im container 
cat 10-calico.conflist | jq
```
