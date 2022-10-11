# Kubectl unter windows installieren (microk8s) 

## Walkthrough (Installation)

```
# Step 1
chocolatry installiert. 
(powershell als Administrator ausführen)
# https://docs.chocolatey.org/en-us/choco/setup
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Step 2
choco install kubernetes-cli 

# Step 3
testen:
kubectl version --client 

# Step 4:
# powershell als normaler benutzer öffnen 
```

## Walkthrough (autocompletion) 

```
in powershell (normaler Benutzer) 
kubectl completion powershell | Out-String | Invoke-Expression
```

## Walkthrough (kubectl mit Cluster verheiraten) 

```
# in powershell im heimatordner des Benutzers .kube - ordnern anlegen
mkdir .kube 
cd .kube 
# config - datei anlegen (ohne endung und inhalt aus microk8s config dort speichern
# Wichtig: Evtl. IP-Addresse ändern (bei virtualbox) ... muss von aussen erreichbar sein. 
# z.B. vom Windows Client aus 
```


  * https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/
