# Self-Service Stack ausrollen (pro Teilnehmer) 

  * ausgerollt mit terraform (binary ist installiert) - snap install --classic terraform 
  * beinhaltet
      1. 1 controlplane
      1. 2 worker nodes
  * Netzwerk steht noch nicht zur Verfügung 
   
## Walktrough 

```
cd
git clone https://github.com/jmetzger/training-kubernets-networking-stack-do-terraform.git install
cd install
cat /tmp/.env
source /tmp/.env
export TF_VAR_do_token
terraform init
terraform apply -auto-approve
```

## Hinweis

```
# Sollte es nicht sauber durchlaufen
# einfach nochmal
terraform apply -auto-approve

# Wenn das nicht geht, einfach nochmal neu
terraform destroy -auto-approve
terraform apply -auto-approve
```
