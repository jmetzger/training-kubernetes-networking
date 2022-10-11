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

## Reference 

  * https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/
