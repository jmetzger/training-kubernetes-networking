# Options for helm -> kustomize bundle 

## Option 1: helm chart entpacken und das helm chart patchen 

```
helm add repo bitnami .... 
helm template --base-directory=base bitnami/mysql
# patchen kustomize 
kustomize build overlay/prod 
kubectl apply -k overlay/prod 
```

## Option 2: packe helm chart aus 

```
# pull 
helm pull 
tar xvf mysql-9.0.34.tgz 
# templates werden 
kubectl kustomize build overlay/prod
helm install mysql-release mysql # 2. mysql wäre das chart-verzeichnis lokal im filesystem 
# Vorteile 
# ich kann das ganze auch wieder so installieren
# ich kann ein update durch führen 
```

## Option 3: helm --post-renderer 

```
# erst wird template erstellt und dann dann ein weiteres script ausgeführt 
# und dann erst installiert. 
helm install --post-renderer=./patch.sh 

# im shell-script
# kubectl kustomize 
# https://austindewey.com/2020/07/27/patch-any-helm-chart-template-using-a-kustomize-post-renderer/
```

## Option 4: kustomize lädt helm - chart 

```
# kustomization.yml 
https://github.com/kubernetes-sigs/kustomize/blob/master/examples/chart.md
```
