# DNS Resolution of services 

## What is resolved 

```
svc-nginx
svc-nginx.jochen # svc-nginx.<namespace>
svc-nginx.jochen.svc
svc-nginx.jochen.svc.cluster.local 
```

## Exercise 

```
kubectl run podtest --rm -ti --image busybox
If you don't see a command prompt, try pressing enter.
/ # wget -O - http://apple-service.jochen
Connecting to apple-service.jochen (10.245.39.214:80)
writing to stdout
apple-tln1
-                    100% |**************************************************************************************************************|    11  0:00:00 ETA
written to stdout
/ # wget -O - http://apple-service.jochen.svc.cluster.local
Connecting to apple-service.jochen.svc.cluster.local (10.245.39.214:80)
writing to stdout
apple-tln1
-                    100% |**************************************************************************************************************|    11  0:00:00 ETA
written to stdout
/ # wget -O - http://apple-service
Connecting to apple-service (10.245.39.214:80)
writing to stdout
apple-tln1
-                    100% |**************************************************************************************************************|    11  0:00:00 ETA
written to stdout
```


## How to find the FQDN (Full qualified domain name) 

```
# in busybox (clusterIP)
nslookup 10.109.6.53
name = svc-nginx.jochen.svc.cluster.local
```
