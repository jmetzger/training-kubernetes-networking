# Install istio and install Addons (Kialia, Prometheus a.s.o) 

## On the client (where you also use kubectl) 

### Steps 1: Download install and run 

```
# as tlnx - user 
# find a decent where to run the installation
# not perfect, but better than to put it in home-folder
cd 
mkdir -p manifests/istio
cd manifests/istio
```

```
# now download the install an run the shell
curl -L https://istio.io/downloadIstio | sh -
```

## Step 2: Run istioctl - commands (version-check, precheck and install) 

```
# This istioctl will be under istio-1.20.2/bin
# but TRAINER has already installed it under /usr/bin/istioctl
# So we can use that one !! 
```

```
# cd istio-1.20.2/bin
istioctl version
istioctl x precheck 
istioctl install --set profile=demo -y
```

## Step 3: Install the addons 

```
# Install Add-Ons
kubectl apply -f istio-1.20.2/samples/addons/ 
```

## Step 4: Check if all the corresponding container (from istio and addons) are running 

```
kubectl -n istio-system get pods 
```
