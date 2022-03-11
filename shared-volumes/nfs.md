# SharedVolume with NFS

## Create new server and install nfs-server

```
# on Ubuntu 20.04LTS
apt install nfs-kernel-server 
systemctl status nfs-server 

vi /etc/exports 
# adjust ip's of kubernetes master and nodes 
# kmaster
/var/nfs/ 192.168.56.101(rw,sync,no_root_squash,no_subtree_check)
# knode1
/var/nfs/ 192.168.56.103(rw,sync,no_root_squash,no_subtree_check)
# knode 2
/var/nfs/ 192.168.56.105(rw,sync,no_root_squash,no_subtree_check)

exportfs -av 
```

## On all clients 

```
### Please do this on all servers 

apt install nfs-common 
# for testing 
mkdir /mnt/nfs 
# 192.168.56.106 is our nfs-server 
mount -t nfs 192.168.56.106:/var/nfs /mnt/nfs 
ls -la /mnt/nfs
umount /mnt/nfs
```

## Setup PersistentVolume and PersistentVolumeClaim in cluster

```
# vi nfs.yml 
apiVersion: v1
kind: PersistentVolume
metadata:
  # any PV name
  name: pv-nfs
  labels:
    volume: nfs-data-volume
spec:
  capacity:
    # storage size
    storage: 5Gi
  accessModes:
    # ReadWriteMany(RW from multi nodes), ReadWriteOnce(RW from a node), ReadOnlyMany(R from multi nodes)
    - ReadWriteMany
  persistentVolumeReclaimPolicy:
    # retain even if pods terminate
    Retain
  nfs:
    # NFS server's definition
    path: /var/nfs/nginx
    server: 192.168.56.106
    readOnly: false
  storageClassName: ""
---
# now we want to claim space
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv-nfs-claim
spec:
  storageClassName: ""
  volumeName: pv-nfs
  accessModes:
  - ReadWriteMany
  resources:
     requests:
       storage: 1Gi
```
```
kubectl apply -f nfs.yml
```

```
# deployment including mount 
# vi deploy.yml 
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 4 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
    
      volumes:
      - name: nfsvol
        persistentVolumeClaim:
          claimName: pv-nfs-claim
    
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        livenessProbe:
          exec:
            command:
            - /bin/sh
            - -c
            - "[ -f /run/nginx.pid ]"
          initialDelaySeconds: 10
          periodSeconds: 5

        readinessProbe:
          httpGet:
            scheme: HTTP
            path: /
            port: 80
          initialDelaySeconds: 10
          periodSeconds: 5

        volumeMounts:
          - mountPath: "/usr/share/nginx/html"
     
```

```
kubectl apply -f deploy.yml 

```
