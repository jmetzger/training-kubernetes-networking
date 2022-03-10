# Kubernetes Native Backup (kasten.io) 

## Background 

  * Belongs to veeam (one of the major companies for backup software) 

## What does Kubernetes Native Backup mean ? 

  * It is tight into the control plane, so it knows about the objects 
  * Uses the api to find out about Kubernetes 

## Setup a storage class (Where to store backup) 

  * https://docs.kasten.io/latest/install/storage.html#direct-provider-integration

## Inject backup into a namespace to be used by app 

  * https://docs.kasten.io/latest/install/generic.html#using-sidecars

## Restore:

```
Restore is done on the K10 - Interface 
```

## Creating MYSQL - Backup / Restore with Kasten 

  * TODO: maybe move this to a seperate page 
  * https://blog.kasten.io/kubernetes-backup-and-restore-for-mysql


## Ref: 

  * https://www.kasten.io 
  * [Installation DigitalOcean](https://docs.kasten.io/install/digitalocean/digitalocean.html)
  * [Installation Kubernetes (Other distributions)](https://docs.kasten.io/install/other/other.html#prerequisites)

