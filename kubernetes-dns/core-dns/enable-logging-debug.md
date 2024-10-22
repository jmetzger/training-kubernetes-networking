# Enable logging / debugging in CoreDNS 

## Step 1: Change the config file

```
kubectl -n kube-system edit cm coredns 
```

```
# Add the following to lines at the beginning
log
debug
```

```
# Now parts of your configfile will look like this
apiVersion: v1
data:
  Corefile: |
    .:53 {
        log
        debug
        errors
        health {
           lameduck 5s
        }
        ready
        kubernetes cluster.local in-addr.arpa ip6.arpa {
           pods insecure
           fallthrough in-addr.arpa ip6.arpa
           ttl 30
        }
        prometheus :9153
        forward . /etc/resolv.conf {
           max_concurrent 1000
        }
        cache 30
        loop
        reload
        loadbalance
    }
kind: ConfigMap
```

```
# In this case, the change is reflected immediately
```

## Step 2: Find out if the change was done (kubectl >= 1.30)



  * Newer versions (kubectl >= 1.30) of kubernetes do have the --custom feature in kubectl debug

```
# In kubernetes 1.30 it is still alpha and you need to activate it
KUBECTL_DEBUG_CUSTOM_PROFILE=true kubectl debug -it <POD_NAMe> --image=<DEBUG_CONTAINER_IMAGE> --target=<TARGET_CONTAINER> --custom="<PATH_TO_CUSTOM_DEBUG_PROFILE_FROM_ABOVE>" 
# In kubernetes 1.31 you can simply use it
```

  * Here is how it works (on/with your kubectl client)

```
cd
mkdir -p manifests 
cd manifests
mkdir coredns
cd coredns
```

```
nano debug.json
```

```
{
  "volumeMounts": [
     {
       "mountPath": "/etc/coredns",
       "name": "config-volume",
       "readOnly": true
     }
  ],
  "env": [
    {
      "name": "TESTME",
      "value": "value1"
    },
    {
      "name": "TESTIT",
      "value": "value2"
    }
  ]
}
```

```
kubectl -n kube-system debug -it coredns-7c65d6cfc9-mqpn8 --custom=debug.json --image=busybox
```

```
# in debug containe
# and it is there - no need to restart pod 
cat /etc/coredns/Corefile
```
