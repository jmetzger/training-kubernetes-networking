# Kubernetes und Docker Administration und Orchestrierung 


## Agenda
  1. Docker-Grundlagen 
     * [Übersicht Architektur](#übersicht-architektur)
     * [Was ist ein Container ?](#was-ist-ein-container-)
     * [Was sind container images](#was-sind-container-images)
     * [Container vs. Virtuelle Maschine](#container-vs-virtuelle-maschine)
     * [Was ist ein Dockerfile](#was-ist-ein-dockerfile)
  
  1. Docker-Installation
     * [BEST for Ubuntu - Install Docker from Docker Repo](#best-for-ubuntu---install-docker-from-docker-repo)
     * [Installation Docker unter Ubuntu mit snap](#installation-docker-unter-ubuntu-mit-snap)
     * [Installation Docker unter SLES 15](#installation-docker-unter-sles-15)
  
  1. Docker-Befehle 
     * [Die wichtigsten Befehle](#die-wichtigsten-befehle)
     * [Logs anschauen - docker logs - mit Beispiel nginx](#logs-anschauen---docker-logs---mit-beispiel-nginx)
     * [docker run](#docker-run)
     * [Docker container/image stoppen/löschen](#docker-containerimage-stoppenlöschen)
     * [Docker containerliste anzeigen](#docker-containerliste-anzeigen)
     * [Docker nicht verwendete Images/Container löschen](#docker-nicht-verwendete-imagescontainer-löschen)
     * [Docker container analysieren](#docker-container-analysieren)
     * [Docker container in den Vordergrund bringen - attach](#docker-container-in-den-vordergrund-bringen---attach)
     * [Aufräumen - container und images löschen](#aufräumen---container-und-images-löschen)
     * [Nginx mit portfreigabe laufen lassen](#nginx-mit-portfreigabe-laufen-lassen)
  
  1. Dockerfile - Examples 
     * [Ubuntu mit hello world](#ubuntu-mit-hello-world)
     * [Ubuntu mit ping](#ubuntu-mit-ping)
     * [Nginx mit content aus html-ordner](#nginx-mit-content-aus-html-ordner)
     * [ssh server](#ssh-server)
  
  1. Docker-Container Examples 
     * [2 Container mit Netzwerk anpingen](#2-container-mit-netzwerk-anpingen)
     * [Container mit eigenem privatem Netz erstellen](#container-mit-eigenem-privatem-netz-erstellen)
  
  1. Docker-Daten persistent machen / Shared Volumes 
     * [Überblick](#überblick)
     * [Volumes](#volumes)
     * [bind-mounts](#bind-mounts)
  
  1. Docker-Netzwerk 
     * [Netzwerk](#netzwerk)
     
  1. Docker Security 
     * [Docker Security](#docker-security)
     * [Scanning docker image with docker scan/snyx](#scanning-docker-image-with-docker-scansnyx)
  
  1. Docker Compose
     * [yaml-format](#yaml-format)
     * [Ist docker-compose installiert?](#ist-docker-compose-installiert)
     * [Example with Wordpress / MySQL](#example-with-wordpress--mysql)
     * [Example with Wordpress / Nginx / MariadB](#example-with-wordpress--nginx--mariadb)
     * [Example with Ubuntu and Dockerfile](#example-with-ubuntu-and-dockerfile)
     * [Logs in docker - compose](#logs-in-docker---compose)
     * [docker-compose und replicas](#docker-compose-und-replicas)
     * [docker compose Reference](https://docs.docker.com/compose/compose-file/compose-file-v3/)
  
  1. Docker Swarm 
     * [Docker Swarm Beispiele](#docker-swarm-beispiele)

  1. Docker - Dokumentation 
     * [Vulnerability Scanner with docker](https://docs.docker.com/engine/scan/#prerequisites)
     * [Vulnerability Scanner mit snyk](https://snyk.io/plans/)
     * [Parent/Base - Image bauen für Docker](https://docs.docker.com/develop/develop-images/baseimages/)
    
  1. Kubernetes - Überblick
     * [Warum Kubernetes, was macht Kubernetes](#warum-kubernetes-was-macht-kubernetes)
     * [Aufbau Allgemein](#aufbau-allgemein)
     * [Aufbau mit helm,OpenShift,Rancher(RKE),microk8s](#aufbau-mit-helmopenshiftrancherrkemicrok8s)
     * [Welches System ? (minikube, micro8ks etc.)](#welches-system--minikube-micro8ks-etc)
     * [Installation - Welche Komponenten from scratch](#installation---welche-komponenten-from-scratch)

  1. Kubernetes - microk8s (Installation und Management) 
     * [Installation Ubuntu - snap](#installation-ubuntu---snap)
     * [Patch to next major release - cluster](#patch-to-next-major-release---cluster)
     * [Remote-Verbindung zu Kubernetes (microk8s) einrichten](#remote-verbindung-zu-kubernetes-microk8s-einrichten)
     * [Bash completion installieren](#bash-completion-installieren)
     * [vim support for yaml](#vim-support-for-yaml)
     * [kubectl unter windows - Remote-Verbindung zu Kuberenets (microk8s) einrichten](#kubectl-unter-windows---remote-verbindung-zu-kuberenets-microk8s-einrichten)
     * [Create a cluster with microk8s](#create-a-cluster-with-microk8s)
     * [Ingress controller in microk8s aktivieren](#ingress-controller-in-microk8s-aktivieren)
     * [Arbeiten mit der Registry](#arbeiten-mit-der-registry)
     * [Installation Kubernetes Dashboard](#installation-kubernetes-dashboard)

  1. Kubernetes Praxis API-Objekte 
     * [Das Tool kubectl (Devs/Ops) - Spickzettel](#das-tool-kubectl-devsops---spickzettel)
     * [kubectl example with run](#kubectl-example-with-run)
     * [Bauen einer Applikation mit Resource Objekten](#bauen-einer-applikation-mit-resource-objekten)
     * [kubectl/manifest/pod](#kubectlmanifestpod)
     * ReplicaSets (Theorie) - (Devs/Ops)
     * [kubectl/manifest/replicaset](#kubectlmanifestreplicaset)
     * Deployments (Devs/Ops)
     * [kubectl/manifest/deployments](#kubectlmanifestdeployments)
     * [Services - Aufbau](#services---aufbau)
     * [kubectl/manifest/service](#kubectlmanifestservice)
     * DaemonSets (Devs/Ops)
     * IngressController (Devs/Ops)
     * [Hintergrund Ingress](#hintergrund-ingress)
     * [Ingress Controller auf Digitalocean (doks) mit helm installieren](#ingress-controller-auf-digitalocean-doks-mit-helm-installieren)
     * [Documentation for default ingress nginx](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/)
     * [Beispiel Ingress](#beispiel-ingress)
     * [Install Ingress On Digitalocean DOKS](#install-ingress-on-digitalocean-doks)
     * [Beispiel mit Hostnamen](#beispiel-mit-hostnamen)
     * [Achtung: Ingress mit Helm - annotations](#achtung-ingress-mit-helm---annotations)
     * [Permanente Weiterleitung mit Ingress](#permanente-weiterleitung-mit-ingress)
     * [ConfigMap Example](#configmap-example)
     * [Configmap MariadDB - Example](#configmap-mariaddb---example)
     
  1. Helm (Kubernetes Paketmanager) 
     * [Helm Grundlagen](#helm-grundlagen)
     * [Helm Warum ?](#helm-warum-)
     * [Helm Example](#helm-example)

  1. Kubernetes - RBAC 
     * [Nutzer einrichten](#nutzer-einrichten)
 
  1. Kubernetes - Netzwerk (CNI's) / Mesh
     * [Übersicht Netzwerke](#übersicht-netzwerke)
     * [Callico - nginx example](#callico---nginx-example)
     * [Callico - client-backend-ui-example](#callico---client-backend-ui-example)
     * [Mesh / istio](#mesh--istio)
   
  1. kubectl 
     * [Start pod (container with run && examples)](#start-pod-container-with-run--examples)
     * [Bash completion for kubectl](#bash-completion-for-kubectl)
     * [kubectl Spickzettel](#kubectl-spickzettel)
     * [Tipps&Tricks zu Deploymnent - Rollout](#tippstricks-zu-deploymnent---rollout)
     
  1. kubectl - manifest - examples 
     * [02 Pod nginx mit Port und IP innerhalb des Clusters](#02-pod-nginx-mit-port-und-ip-innerhalb-des-clusters)
     * [03b Example with service and nginx](#03b-example-with-service-and-nginx)
     * [04 Ingress mit einfachem Beispiel](#04-ingress-mit-einfachem-beispiel)
     * [05 Ingress mit Permanent Redirect](#05-ingress-mit-permanent-redirect)

  1. Kubernetes - Monitoring (microk8s und vanilla) 
     * [metrics-server aktivieren (microk8s und vanilla)](#metrics-server-aktivieren-microk8s-und-vanilla)

  1. Kubernetes - Shared Volumes 
     * [Shared Volumes with nfs](#shared-volumes-with-nfs)

  1. Kubernetes - Backups 
     + [Kubernetes Aware Cloud Backup - kasten.io](/backups/cluster-backup-kasten-io.md)

  1. Kubernetes - Wartung / Debugging 
     * [kubectl drain/uncordon](#kubectl-drainuncordon)
     * [Alte manifeste konvertieren mit convert plugin](#alte-manifeste-konvertieren-mit-convert-plugin)
     * [Netzwerkverbindung zu pod testen](#netzwerkverbindung-zu-pod-testen)

  1. Kubernetes - Tipps & Tricks 
     * [Assigning Pods to Nodes](#assigning-pods-to-nodes)
     * [Kubernetes Debuggen ClusterIP/PodIP](#kubernetes-debuggen-clusterippodip)
     * [Debugging pods](#debugging-pods)
     * [Autoscaling Pods/Deployments](#autoscaling-podsdeployments)

  1. Kubernetes - Documentation 
     * [Documentation zu microk8s plugins/addons](https://microk8s.io/docs/addons)
     * [LDAP-Anbindung](https://github.com/apprenda-kismatic/kubernetes-ldap)
     * [Shared Volumes - Welche gibt es ?](https://kubernetes.io/docs/concepts/storage/volumes/)
     * [Helpful to learn - Kubernetes](https://kubernetes.io/docs/tasks/)
     * [Environment to learn](https://killercoda.com/killer-shell-cks)
     * [Environment to learn II](https://killercoda.com/)
     * [Youtube Channel](https://www.youtube.com/watch?v=01qcYSck1c4)

  1. Kubernetes -Wann / Wann nicht 
     * [Kubernetes Wann / Wann nicht](#kubernetes-wann--wann-nicht)

  1. Kubernetes - Hardening 
     * [Kubernetes Tipps Hardening](#kubernetes-tipps-hardening)

  1. Kubernetes Deployment Scenarios 
     * [Deployment green/blue,canary,rolling update](#deployment-greenbluecanaryrolling-update)
     * [Praxis-Übung A/B Deployment](#praxis-übung-ab-deployment)

  1. Kubernetes Probes (Liveness and Readiness) 
     * [Übung Liveness-Probe](#übung-liveness-probe)
     * [Funktionsweise Readiness-Probe vs. Liveness-Probe](#funktionsweise-readiness-probe-vs-liveness-probe)
     
  1. Kubernetes Interna 
     * [OCI,Container,Images Standards](#ocicontainerimages-standards)
   
  1. Linux und Docker Tipps & Tricks allgemein 
     * [Auf ubuntu root-benutzer werden](#auf-ubuntu-root-benutzer-werden)
     * [IP - Adresse abfragen](#ip---adresse-abfragen)
     * [Hostname setzen](#hostname-setzen)
     * [Proxy für Docker setzen](#proxy-für-docker-setzen)
     * [vim einrückung für yaml-dateien](#vim-einrückung-für-yaml-dateien)
     * [YAML Linter Online](http://www.yamllint.com/)
     * [Läuft der ssh-server](#läuft-der-ssh-server)
     * [Basis/Parent - Image erstellen](#basisparent---image-erstellen)
     * [Eigenes unsichere Registry-Verwenden. ohne https](#eigenes-unsichere-registry-verwenden-ohne-https)
     
  1. VirtualBox Tipps & Tricks 
     * [VirtualBox 6.1. - Ubuntu für Kubernetes aufsetzen ](#virtualbox-61---ubuntu-für-kubernetes-aufsetzen-)
     * [VirtualBox 6.1. - Shared folder aktivieren](#virtualbox-61---shared-folder-aktivieren)

<div class="page-break"></div>

## Docker-Grundlagen 

### Übersicht Architektur


![Docker Architecture - copyright geekflare](https://geekflare.com/wp-content/uploads/2019/09/docker-architecture-609x270.png)

### Was ist ein Container ?


```
- vereint in sich Software
- Bibliotheken 
- Tools 
- Konfigurationsdateien 
- keinen eigenen Kernel 
- gut zum Ausführen von Anwendungen auf verschiedenen Umgebungen 

- Container sind entkoppelt
- Container sind voneinander unabhängig 
- Können über wohldefinierte Kommunikationskanäle untereinander Informationen austauschen

- Durch Entkopplung von Containern:
  o Unverträglichkeiten von Bibliotheken, Tools oder Datenbank können umgangen werden, wenn diese von den Applikationen in unterschiedlichen Versionen benötigt werden.
```

### Was sind container images


  * Container Image benötigt, um zur Laufzeit Container-Instanzen zu erzeugen 
  * Bei Docker werden Docker Images zu Docker Containern, wenn Sie auf einer Docker Engine als Prozess ausgeführt werden.
  * Man kann sich ein Docker Image als Kopiervorlage vorstellen.
    * Diese wird genutzt, um damit einen Docker Container als Kopie zu erstellen   

### Container vs. Virtuelle Maschine


```
VM's virtualisieren Hardware
Container virtualisieren Betriebssystem 


```

### Was ist ein Dockerfile


## What is it ? 
 * Textdatei, die Linux - Kommandos enthält
   * die man auch auf der Kommandozeile ausführen könnte 
   * Diese erledigen alle Aufgaben, die nötig sind, um ein Image zusammenzustellen
   * mit docker build wird dieses image erstellt 
   
## Exmaple 

```
## syntax=docker/dockerfile:1
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

## Docker-Installation

### BEST for Ubuntu - Install Docker from Docker Repo


### Walkthrough 

```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### Läuft der Dienst (dockerd) 

```
systemctl status docker 
```

### docker-compose ? 

```
## herausfinden, ob docker compose installieren 
docker compose version 
```

### Installation Docker unter Ubuntu mit snap


```

sudo su -
snap install docker

## for information retrieval 
snap info docker
systemctl list-units
systemctl list-units -t service
systemctl list-units -t service | grep docker

systemctl status snap.docker.dockerd.service
## oder (aber veraltet) 
service snap.docker.dockerd status

systemctl stop snap.docker.dockerd.service
systemctl status snap.docker.dockerd.service
systemctl start snap.docker.dockerd.service 

## wird der docker-dienst beim nächsten reboot oder starten des Server gestartet ? 
systemctl is-enabled snap.docker.dockerd.service

```

### Installation Docker unter SLES 15


### Walkthrough 


```
sudo zypper search -v docker*
sudo zypper install docker

## Dem Nutzer /z.B. Nutzer kurs die Gruppe docker hinzufügen 
## damit auch dieser den Docker-daemon verwenden darf 
sudo groupadd docker
sudo usermod -aG docker $USER

### Unter SLES werden Dienste nicht automatisch aktiviert und gestartet !!! 
## Service für start nach Boot aktivieren 
newgrp docker 
sudo systemctl enable docker.service
## Docker dienst starten 
sudo systemctl start docker.service
```


### Ausführlich mit Ausgaben 

```
sudo zypper search -v docker*

Repository-Daten werden geladen...
Installierte Pakete werden gelesen...

sudo zypper install docker

Dienst 'Basesystem_Module_x86_64' wird aktualisiert.
Dienst 'Containers_Module_x86_64' wird aktualisiert.
Dienst 'Desktop_Applications_Module_x86_64' wird aktualisiert.
Dienst 'Development_Tools_Module_x86_64' wird aktualisiert.
Dienst 'SUSE_Linux_Enterprise_Server_x86_64' wird aktualisiert.
Dienst 'Server_Applications_Module_x86_64' wird aktualisiert.
Repository-Daten werden geladen...
Installierte Pakete werden gelesen...
Paketabhängigkeiten werden aufgelöst...

Das folgende empfohlene Paket wurde automatisch gewählt:
  git-core

Die folgenden 7 NEUEN Pakete werden installiert:
  catatonit containerd docker docker-bash-completion git-core libsha1detectcoll1 runc

7 neue Pakete zu installieren.
Gesamtgröße des Downloads: 52,2 MiB. Bereits im Cache gespeichert: 0 B. Nach der Operation werden zusätzlich 242,1 MiB belegt.
Fortfahren? [j/n/v/...? zeigt alle Optionen](j): j
Paket libsha1detectcoll1-1.0.3-2.18.x86_64 abrufen                                                                                                                                                                          (1/7),  23,2 KiB ( 45,8 KiB entpackt)
Abrufen: libsha1detectcoll1-1.0.3-2.18.x86_64.rpm .......................................................................................................................................................................................................[fertig]
Paket catatonit-0.1.5-3.3.2.x86_64 abrufen                                                                                                                                                                                  (2/7), 257,2 KiB (696,5 KiB entpackt)
Abrufen: catatonit-0.1.5-3.3.2.x86_64.rpm ...............................................................................................................................................................................................................[fertig]
Paket runc-1.1.4-150000.33.4.x86_64 abrufen                                                                                                                                                                                 (3/7),   2,6 MiB (  9,1 MiB entpackt)
Abrufen: runc-1.1.4-150000.33.4.x86_64.rpm ..............................................................................................................................................................................................................[fertig]
Paket containerd-1.6.6-150000.73.2.x86_64 abrufen                                                                                                                                                                           (4/7),  17,7 MiB ( 74,2 MiB entpackt)
Abrufen: containerd-1.6.6-150000.73.2.x86_64.rpm ........................................................................................................................................................................................................[fertig]
Paket git-core-2.35.3-150300.10.15.1.x86_64 abrufen                                                                                                                                                                         (5/7),   4,8 MiB ( 26,6 MiB entpackt)
Abrufen: git-core-2.35.3-150300.10.15.1.x86_64.rpm ......................................................................................................................................................................................................[fertig]
Paket docker-20.10.17_ce-150000.166.1.x86_64 abrufen                                                                                                                                                                        (6/7),  26,6 MiB (131,4 MiB entpackt)
Abrufen: docker-20.10.17_ce-150000.166.1.x86_64.rpm .....................................................................................................................................................................................................[fertig]
Paket docker-bash-completion-20.10.17_ce-150000.166.1.noarch abrufen                                                                                                                                                        (7/7), 121,3 KiB (113,6 KiB entpackt)
Abrufen: docker-bash-completion-20.10.17_ce-150000.166.1.noarch.rpm .....................................................................................................................................................................................[fertig]

Überprüfung auf Dateikonflikte läuft: ...................................................................................................................................................................................................................[fertig]
(1/7) Installieren: libsha1detectcoll1-1.0.3-2.18.x86_64 ................................................................................................................................................................................................[fertig]
(2/7) Installieren: catatonit-0.1.5-3.3.2.x86_64 ........................................................................................................................................................................................................[fertig]
(3/7) Installieren: runc-1.1.4-150000.33.4.x86_64 .......................................................................................................................................................................................................[fertig]
(4/7) Installieren: containerd-1.6.6-150000.73.2.x86_64 .................................................................................................................................................................................................[fertig]
(5/7) Installieren: git-core-2.35.3-150300.10.15.1.x86_64 ...............................................................................................................................................................................................[fertig]
Updating /etc/sysconfig/docker ...
(6/7) Installieren: docker-20.10.17_ce-150000.166.1.x86_64 ..............................................................................................................................................................................................[fertig]
(7/7) Installieren: docker-bash-completion-20.10.17_ce-150000.166.1.noarch ..............................................................................................................................................................................[fertig]

sudo groupadd docker
sudo usermod -aG docker $USER
// logout

newgrp docker 
sudo systemctl enable docker.service
sudo systemctl start docker.service
```

## Docker-Befehle 

### Die wichtigsten Befehle


```
## docker hub durchsuchen
docker search hello-world

docker run <image>
## z.b. // Zieht das image aus docker hub 
## hub.docker.com 
docker run hello-world

## images die lokal vorhanden 
docker images 

## container (laufende) 
docker container ls 
## container (vorhanden, aber beendet)
docker container ls -a 

## z.b hilfe für docker run 
docker help run 

 


```

### Logs anschauen - docker logs - mit Beispiel nginx


### Allgemein 
```
## Erstmal nginx starten und container-id wird ausgegeben 
docker run -d nginx 
a234
docker logs a234 # a234 sind die ersten 4 Ziffern der Container ID 
```

### Laufende Log-Ausgabe 

```
docker logs -f a234 
## Abbrechen CTRL + c 
```

### docker run


### Beispiel (binden an ein terminal), detached

```
## before that we did
docker pull ubuntu:xenial
## image vorhanden 
docker images

docker run -t -d --name my_xenial ubuntu:xenial
## will wollen überprüfen, ob der container läuft
docker container ls 


## in den Container reinwechsel 
docker exec -it my_xenial bash 
docker exec my_xenial cat /etc/os-release
## 

```

### Docker container/image stoppen/löschen


```
docker stop ubuntu-container 
## Kill it if it cannot be stopped -be careful
docker kill ubuntu-container

## Get nur, wenn der Container nicht mehr läuft 
docker rm ubuntu-container

## oder alternative
docker rm -f ubuntu-container 


## image löschen 
docker rmi ubuntu:xenial 

## falls Container noch vorhanden aber nicht laufend 
docker rmi -f ubuntu:xenial 

```

### Docker containerliste anzeigen


```
## besser 
docker container ls 
## Alle Container, auch die, die beendet worden sind 
docker container ls -a 


## deprecated 
docker ps 
## -a auch solche die nicht mehr laufen 
docker ps -a



```

### Docker nicht verwendete Images/Container löschen


```
docker system prune 
## Löscht möglicherweise nicht alles

## d.h. danach nochmal prüfen ob noch images da sind
docker images 
## und händisch löschen 
docker rmi <image-name>

```

### Docker container analysieren


```
docker run -t -d --name mein_container ubuntu:latest
docker inspect mein_container # mein_container = container name 
```

### Docker container in den Vordergrund bringen - attach


### docker attach - walkthrough 

```
docker run -d ubuntu 
1a4d...

docker attach 1a4d 

## Es ist leider mit dem Aufruf run nicht möglich, den prozess wieder in den Hintergrund zu bringen 

```

### interactiven Prozess nicht beenden (statt exit) 

```
docker run -it ubuntu bash  
## ein exit würde jetzt den Prozess beenden
## exit

## Alternativ ohne beenden (detach) 
## Geht aber nur beim start mit run -it 
CTRL + P, dann CTRL + Q 

```

### Reference: 

  * https://docs.docker.com/engine/reference/commandline/attach/

### Aufräumen - container und images löschen


### Alle nicht verwendeten container und images löschen 

```
## Alle container, die nicht laufen löschen 
docker container prune 

## Alle images, die nicht an eine container gebunden sind, löschen 
docker images prune 

```

### Nginx mit portfreigabe laufen lassen


```
docker run --name test-nginx -d -p 8080:80 nginx

docker container ls
lsof -i
cat /etc/services | grep 8080
curl http://localhost:8080
docker container ls
## wenn der container gestoppt wird, keine ausgabe mehr, weil kein webserver
docker stop test-nginx 
curl http://localhost:8080


```

## Dockerfile - Examples 

### Ubuntu mit hello world


### Simple Version 

#### Schritt 1:
```
cd 
mkdir hello-world 
cd hello-world
```

#### Schritt 2:

```
## nano Dockerfile
FROM ubuntu:latest 

COPY hello.sh .
RUN chmod u+x hello.sh
CMD ["/hello.sh"]
```

#### Schritt 3:
```
nano hello.sh 
```

```
##!/bin/bash
let i=0

while true
do
  let i=i+1
  echo $i:hello-docker
  sleep 5
done
```

#### Schritt 4:

```
## dockertrainereu/<dein-name>-hello-docker . 
## Beispiel
docker build -t dockertrainereu/jm-hello-docker .
docker images
docker run dockertrainereu/<dein-name>-hello-docker 
```


#### Schritt 5:

```
docker login
user: dockertrainereu 
pass: --bekommt ihr vom trainer--

## docker push dockertrainereu/<dein-name>-hello-docker 
## z.B. 
docker push dockertrainereu/jm-hello-docker

## und wir schauen online, ob wir das dort finden

```



### Ubuntu mit ping


```
mkdir myubuntu 
cd myubuntu/
```

```
## nano Dockerfile
FROM ubuntu:22.04
RUN apt-get update; apt-get install -y inetutils-ping
## CMD ["/bin/bash"]
```

```
## Variante 2
## nano Dockerfile
FROM ubuntu:22.04
RUN apt-get update && \
    apt-get install -y inetutils-ping && \
    rm -rf /var/lib/apt/lists/*
## CMD ["/bin/bash"]
```

```
docker build -t myubuntu .
docker images
## -t wird benötigt, damit bash WEITER im Hintergrund im läuft.
## auch mit -d (ohne -t) wird die bash ausgeführt, aber "das Terminal" dann direkt beendet 
## -> container läuft dann nicht mehr 
```

```
docker run -d -t --name container-ubuntu myubuntu
docker container ls
## in den container reingehen mit dem namen des Containers: myubuntu 
docker exec -it myubuntu bash
ls -la
 ```

```
## Zweiten Container starten
docker run -d -t --name container-ubuntu2 myubuntu 

## docker inspect to find out ip of other container 
## 172.17.0.3 
docker inspect <container-id>

## Ersten Container -> 2. anpingen 
docker exec -it container-ubuntu bash 
## Jeder container hat eine eigene IP 
ping 172.17.0.3

 
```

### Nginx mit content aus html-ordner


### Schritt 1: Simple Example 

```
## das gleich wie cd ~
## Heimatverzeichnis des Benutzers root 
cd
mkdir nginx-test
cd nginx-test
mkdir html
cd html/
vi index.html

```

```
Text, den du rein haben möchtest 
```

```
cd ..
vi Dockerfile 
```

```
FROM nginx:latest
COPY html /usr/share/nginx/html
```

```
## nameskürzel z.B. jm1 
docker build -t nginx-test  . 
docker images

```



### Schritt 2: docker laufen lassen

```
## und direkt aus der Registry wieder runterladen 
docker run --name hello-web -p 8080:80 -d nginx-test

## laufenden Container anzeigen lassen
docker container ls 
## oder alt: deprecated 
docker ps 

curl http://localhost:8080 


## 
docker rm -f hello-web 

```

### ssh server


```
cd 
mkdir devubuntu
cd devubuntu 
## vi Dockerfile 
```

```Dockerfile
FROM ubuntu:latest

RUN apt-get update && \
    DEBIAN_FRONTEND="noninteractive" apt-get install -y inetutils-ping openssh-server && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir /run/sshd && \
    echo 'root:root' | chpasswd && \
    sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config && \
    mkdir /root/.ssh

EXPOSE 22/tcp

CMD ["/usr/sbin/sshd","-D"]
```

```
docker build -t devubuntu . 
docker run --name=devjoy -p 2222:22  -d -t devubuntu3

ssh root@localhost -p 2222
## example, if your docker host ist 192.168.56.101 v
ssh root@192.168.56.101 -p 2222
```

## Docker-Container Examples 

### 2 Container mit Netzwerk anpingen


```
clear
docker run --name dockerserver1 -dit ubuntu
docker run --name dockerserver2 -dit ubuntu
docker network ls
docker network inspect bridge
## dockerserver1 - 172.17.0.2
## dockerserver2 - 172.17.0.3
docker container ls
docker exec -it dockerserver1 bash
## im container 
apt update; apt install -y iputils-ping 
ping 172.17.0.3 
```

### Container mit eigenem privatem Netz erstellen


```
clear
## use bridge as type
## docker network create -d bridge test_net
## by bridge is default 
docker network create test_net
docker network ls
docker network inspect test_net

## Container mit netzwerk starten 
docker container run -d --name nginx1 --network test_net nginx
docker network inspect test_net

## Weiteres Netzwerk (bridged) erstellen
docker network create demo_net
docker network connect demo_net nginx1

## Analyse 
docker network inspect demo_net
docker inspect nginx1

## Verbindung lösen 
docker network disconnect demo_net nginx1

## Schauen, wir das Netz jetzt aussieht 
docker network inspect demo_net

```

## Docker-Daten persistent machen / Shared Volumes 

### Überblick


### Overview 

```
bind-mount  # not recommended 
volumes
tmpfs 
```

### Disadvantags 

```
stored only on one node
Does not work well in cluster


```

### Alternative for cluster 

```
glusterfs
cephfs 
nfs 

## Stichwort
ReadWriteMany 


```

### Volumes


### Storage volumes verwalten 

```
docker volume ls
docker volume create test-vol
docker volume ls
docker volume inspect test-vol
```

### Storage volumes in container einhängen

```
## Schritt 1
docker run -it --name container-test-vol --mount target=/test_data,source=test-vol ubuntu bash
1234ad# touch /test_data/README 
exit
## stops container 
docker container ls -a
```

```
## Schritt 2:
## create new container and check for /test_data/README 
docker run -it --name=container-test-vol2 --mount target=/test_data,source=test-vol ubuntu bash
ab45# ls -la /test_data/README 
```

### Storage volume löschen 

```
## Zunächst container löschen 
docker rm container-test-vol 
docker rm container-test-vol2
docker volume rm test-vol
```

### bind-mounts


```
## andere Verzeichnis als das Heimatverzeichnis von root funktionieren aktuell nicht mit 
## snap install docker 
## wg. des Confinements 
docker run -d -it  --name devtest --mount type=bind,source=/root,target=/app nginx:latest
docker exec -it devtest bash 
/# cd /app 
```

## Docker-Netzwerk 

### Netzwerk


### Übersicht

```
3 Typen 

o none
o bridge (Standard-Netzwerk) 
o host 

### Additionally possible to install
o overlay (needed for multi-node)

```


### Kommandos 

```
## Netzwerk anzeigen 
docker network ls 

## bridge netzwerk anschauen 
## Zeigt auch ip der docker container an  
docker inspect bridge

## im container sehen wir es auch
docker inspect ubuntu-container 

```

### Eigenes Netz erstellen 

```
docker network create -d bridge test_net 
docker network ls 

docker container run -d --name nginx --network test_net nginx
docker container run -d --name nginx_no_net --network none nginx 

docker network inspect none 
docker network inspect test_net 

docker inspect nginx 
docker inspect nginx_no_net 

```

### Netzwerk rausnehmen / hinzufügen 

```
docker network disconnect none nginx_no_net
docker network connect test_net nginx_no_net 

### Das Löschen von Netzwerken ist erst möglich, wenn es keine Endpoints 
### d.h. container die das Netzwerk verwenden 
docker network rm test_net 
```



## Docker Security 

### Docker Security


### Run container under specific user: 

```
## user with id 40000 does not need to exist in container 
docker run -it -u 40000 alpine 

## user kurs needs to exist in container (/etc/passwd) 
docker run -it -u kurs alpine 

```

### Default capabilities 

  * Set everytime a new container is started as default 
  * https://github.com/moby/moby/blob/master/profiles/seccomp/default.json


### Run container with less capabilities 

```
cd
mkdir captest
cd captest 
```

```
nano docker-compose.yml 
```

```
services: 
  nginx:
    image: nginx 
    cap_drop:
      - CHOWN
```

```
docker compose up -d
## start and exits 
docker compose ps 
## 
docker exec -it captest_nginx_1 bash 
##/ touch /tmp/foo; chown 10000 /tmp/foo  

## what happened -> wants to do chown, but it is not allowed 
docker logs captest_nginx_1 

```

```
docker compose down 
```


### Reference:

  * https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html
  * https://www.redhat.com/en/blog/secure-your-containers-one-weird-trick
  * man capabilities

### Scanning docker image with docker scan/snyx


### Prerequisites 

```
You need to be logged in on docker hub with docker login 
(with your account credentials
```


### Example 

```
## Snyk (docker scan) 
docker help scan
docker scan --json --accept-license dockertrainereu/jm-hello-docker  > result.json
```

## Docker Compose

### yaml-format


```
## Kommentare 

## Listen 
- rot
- gruen
- blau 

## Mappings 
Version: 3.7 

## Mappings können auch Listen enthalten 
expose: 
  - "3000"
  - "8000" 

## Verschachtelte Mappings 
build:
  context: .
  labels: 
    label1: "bunt"
    label2: "hell" 

```

### Ist docker-compose installiert?


```
## besser. mehr infos
docker-compose version 
docker-compose --version 

```

## docker compose direkt als plugin für docker 

```
## Installiert man docker in der neuesten 20.10.21 
## existiert docker als plugin und wird anders aufgerufen 
docker compose 
```


### Example with Wordpress / MySQL



### Schritt 1:
```
clear
cd
mkdir wp
cd wp
nano docker-compose.yml
```

### Schritt 2:

```
## docker-compose.yaml
version: "3.8"

services:
  database:
    image: mysql:5.7
    volumes:
      - database_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: mypassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    image: wordpress:latest
    depends_on:
      - database
    ports:
      - 8080:80
    restart: always
    environment:
      WORDPRESS_DB_HOST: database:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    volumes:
      - wordpress_plugins:/var/www/html/wp-content/plugins
      - wordpress_themes:/var/www/html/wp-content/themes
      - wordpress_uploads:/var/www/html/wp-content/uploads

volumes:
  database_data:
  wordpress_plugins:
  wordpress_themes:
  wordpress_uploads:


```

### Schritt 3:

```
docker-compose up -d 
```

### Example with Wordpress / Nginx / MariadB


```
mkdir wp 
cd wp 
## nano docker-compose.yml 
```

```
version: "3.7"

services:
    database:
        image: mysql:5.7
        volumes:
            - database_data:/var/lib/mysql
        restart: always
        environment:
            MYSQL_ROOT_PASSWORD: mypassword
            MYSQL_DATABASE: wordpress
            MYSQL_USER: wordpress
            MYSQL_PASSWORD: wordpress

    wordpress:
        image: wordpress:latest
        depends_on:
            - database
        ports:
            - 8080:80
        restart: always
        environment:
            WORDPRESS_DB_HOST: database:3306
            WORDPRESS_DB_USER: wordpress
            WORDPRESS_DB_PASSWORD: wordpress
        volumes:
            - wordpress_plugins:/var/www/html/wp-content/plugins
            - wordpress_themes:/var/www/html/wp-content/themes
            - wordpress_uploads:/var/www/html/wp-content/uploads
volumes:
    database_data:
    wordpress_plugins:
    wordpress_themes:
    wordpress_uploads:
```


```
### now start the system
docker compose up -d 
### we can do some test if db is reachable 
docker exec -it wp_wordpress_1 bash 
### within shell do 
apt update 
apt-get install -y telnet
## this should work 
telnet database 3306

## and we even have logs
docker compose logs 

## 
docker compose down 
```

### Example with Ubuntu and Dockerfile


### Schritt 1:

```
cd
mkdir bautest
cd bautest 
```

### Schritt 2:

```
## nano docker-compose.yml
version: "3.8"

services:
  myubuntu:
    build: ./myubuntu
    restart: always
```

### Schritt 3:

```
mkdir myubuntu 
cd myubuntu 
```

```
nano hello.sh
```

```
##!/bin/bash
let i=0

while true
do
  let i=i+1
  echo $i:hello-docker
  sleep 5
done

```

```
## nano Dockerfile 
FROM ubuntu:latest
RUN apt-get update; apt-get install -y inetutils-ping
COPY hello.sh .
RUN chmod u+x hello.sh
CMD ["/hello.sh"]

```

### Schritt 4: 


```
cd ../
## wichtig, im docker-compose - Ordner seiend 
##pwd 
##~/bautest
docker compose up -d 
## wird image gebaut und container gestartet 

## Bei Veränderung vom Dockerfile, muss man den Parameter --build mitangeben 
docker compose up -d --build 
```

### Logs in docker - compose


```
##Im Ordner des Projektes
##z.B wordpress-mysql-compose-project 
cd ~/wordpress-mysql-compose-project 
docker-compose logs
## jetzt werden alle logs aller services angezeigt 
```

### docker-compose und replicas


### Beispiel 

```
version: "3.9"
services:
  redis:
    image: redis:latest
    deploy:
      replicas: 1
    configs:
      - my_config
      - my_other_config
configs:
  my_config:
    file: ./my_config.txt
  my_other_config:
    external: true
```
### Ref:

  * https://docs.docker.com/compose/compose-file/compose-file-v3/

### docker compose Reference

  * https://docs.docker.com/compose/compose-file/compose-file-v3/

## Docker Swarm 

### Docker Swarm Beispiele


### Generic examples 

```
## should be at least version 1.24 
docker info

## only for one network interface
docker swarm init

## in our case, we need to decide what interface
docker swarm init --advertise-addr 192.168.56.101

## is swarm active 
docker info | grep -i swarm
## When it is -> node command works 
docker node ls
## is the current node the manager 
docker info | grep -i "is manager"

## docker create additional overlay network 
docker network ls

## what about my own node -> self
docker node inspect self
docker node inspect --pretty self
docker node inspect --pretty self | less

```

```
## Create our first service 
docker service create redis
docker images
docker service ls
## if service-id start with  j 
docker service inspect j
docker service ps j
docker service rm j
docker service ls
```

```
## Start with multiple replicas and name 
docker service create --name my_redis --replicas 4 redis
docker service ls
## Welche tasks 
docker service ps my_redis
docker container ls
docker service inspect my_redis

## delete service
docker service rm
```

### Add additional node 

```
## on first node, get join token 
docker swarm join-token manager

## on second node execute join command
docker swarm join --token SWMTKN-1-07jy3ym29au7u3isf1hfhgd7wpfggc1nia2kwtqfnfc8hxfczw-2kuhwlnr9i0nkje8lz437d2d5 192.168.56.101:2377

## check with node command
docker node ls 

## Make node a simple worker
## Does not make, because no highavailable after crush node 1
## Take at LEAST 3 NODES 
docker node demote <node-name>

```

### expose port

```
docker service create --name my_web \
                        --replicas 3 \
                        --publish published=8080,target=80 \
                        nginx
```

### Ref 

  * https://docs.docker.com/engine/swarm/services/

## Docker - Dokumentation 

### Vulnerability Scanner with docker

  * https://docs.docker.com/engine/scan/#prerequisites

### Vulnerability Scanner mit snyk

  * https://snyk.io/plans/

### Parent/Base - Image bauen für Docker

  * https://docs.docker.com/develop/develop-images/baseimages/

## Kubernetes - Überblick

### Warum Kubernetes, was macht Kubernetes


  * Virtualisierung von Hardware - 5fache bessere Auslastung
  * Google als Ausgangspunkt 
  * Software 2014 als OpenSource zur Verfügung gestellt 
  * Optimale Ausnutzung der Hardware, hunderte bis tausende Dienste können auf einigen Maschinen laufen (Cluster)  
  * Immutable - System
  * Selbstheilend
  
## Wozu dient Kubernetes ?

  * Orchestrierung von Containern
  * am gebräuchlisten aktuell Docker

### Aufbau Allgemein


### Schaubild 

![Kubernetes Architecture - src: syseleven](https://www.syseleven.de/wp-content/uploads/2020/11/syseleven-webiste-loesungen-kubernetes-modell-800x400-web.jpg)

### Komponenten / Grundbegriffe

#### Master (Control Plane)

##### Aufgaben 

  * Der Master koordiniert den Cluster
  * Der Master koordiniert alle Aktivitäten in Ihrem Cluster
    * Planen von Anwendungen
    * Verwalten des gewünschten Status der Anwendungen
    * Skalieren von Anwendungen
    * Rollout neuer Updates.

##### Komponenten des Masters 

###### etcd

  * Verwalten der Konfiguration des Clusters (key/value - pairs) 
  
###### kube-controller-manager  
  
  * Zuständig für die Überwachung der Stati im Cluster mit Hilfe von endlos loops. 
  * kommuniziert mit dem Cluster über die kubernetes-api (bereitgestellt vom kube-api-server)

###### kube-api-server 

  * provides api-frontend for administration (no gui)
  * Exposes an HTTP API (users, parts of the cluster and external components communicate with it)
  * REST API
 
###### kube-scheduler 

  * assigns Pods to Nodes. 
  * scheduler determines which Nodes are valid placements for each Pod in the scheduling queue 
    ( according to constraints and available resources )
  * The scheduler then ranks each valid Node and binds the Pod to a suitable Node. 
  * Reference implementation (other schedulers can be used)
 
#### Nodes  

  * Nodes (Knoten) sind die Arbeiter (Maschinen), die Anwendungen ausführen
  * Ref: https://kubernetes.io/de/docs/concepts/architecture/nodes/

#### Pod/Pods 

  * Pods sind die kleinsten einsetzbaren Einheiten, die in Kubernetes erstellt und verwaltet werden können.
  * Ein Pod (übersetzt Gruppe) ist eine Gruppe von einem oder mehreren Containern
    * gemeinsam genutzter Speicher- und Netzwerkressourcen   
    * Befinden sich immer auf dem gleich virtuellen Server 
    
### Control Plane Node (former: master) - components 

### Node (Minion) - components 

#### General 

  * On the nodes we will rollout the applications

#### kubelet

```
Node Agent that runs on every node (worker) 
Er stellt sicher, dass Container in einem Pod ausgeführt werden.
```

#### Kube-proxy 

  * Läuft auf jedem Node 
  * = Netzwerk-Proxy für die Kubernetes-Netzwerk-Services.
  * Kube-proxy verwaltet die Netzwerkkommunikation innerhalb oder außerhalb Ihres Clusters.
  
### Referenzen 

  * https://www.redhat.com/de/topics/containers/kubernetes-architecture


### Aufbau mit helm,OpenShift,Rancher(RKE),microk8s


![Aufbau](/images/aufbau-komponente-kubernetes.png)

### Welches System ? (minikube, micro8ks etc.)


## Überblick der Systeme 

### General 

```
kubernetes itself has not convenient way of doing specific stuff like 
creating the kubernetes cluster.

So there are other tools/distri around helping you with that.

```

### Kubeadm

#### General 

  * The official CNCF (https://www.cncf.io/) tool for provisioning Kubernetes clusters
    (variety of shapes and forms (e.g. single-node, multi-node, HA, self-hosted))
  * Most manual way to create and manage a cluster 

#### Disadvantages 

  * Plugins sind oftmals etwas schwierig zu aktivieren

### microk8s 

#### General

  * Created by Canonical (Ubuntu)
  * Runs on Linux
  * Runs only as snap
  * In the meantime it is also available for Windows/Mac
  * HA-Cluster 

#### Production-Ready ? 

  * Short answer: YES 

```
Quote canonical (2020):

MicroK8s is a powerful, lightweight, reliable production-ready Kubernetes distribution. It is an enterprise-grade Kubernetes distribution that has a small disk and memory footprint while offering carefully selected add-ons out-the-box, such as Istio, Knative, Grafana, Cilium and more. Whether you are running a production environment or interested in exploring K8s, MicroK8s serves your needs.

Ref: https://ubuntu.com/blog/introduction-to-microk8s-part-1-2

```

#### Advantages

  * Easy to setup HA-Cluster (multi-node control plane)
  * Easy to manage 

### minikube 

#### Disadvantages
  
  * Not usable / intended for production 

#### Advantages 

  * Easy to set up on local systems for testing/development (Laptop, PC) 
  * Multi-Node cluster is possible 
  * Runs und Linux/Windows/Mac
  * Supports plugin (Different name ?)


### k3s



### kind (Kubernetes-In-Docker)

#### General 

  * Runs in docker container 


#### For Production ?

```
Having a footprint, where kubernetes runs within docker 
and the applikations run within docker as docker containers
it is not suitable for production.
```



### Installation - Welche Komponenten from scratch


### Step 1: Server 1 (manuell installiert -> microk8s)

```
## Installation Ubuntu - Server 

## cloud-init script 
## s.u. BASIS (keine Voraussetzung - nur zum Einrichten des Nutzers 11trainingdo per ssh) 

## Server 1 - manuell 
## Ubuntu 20.04 LTS - Grundinstallation 

## minimal Netzwerk - öffentlichen IP 
## nichts besonderes eingerichtet - Standard Digitalocean 

## Standard vo Installation microk8s 
lo               UNKNOWN        127.0.0.1/8 ::1/128
## public ip / interne 
eth0             UP             164.92.255.234/20 10.19.0.6/16 fe80::c:66ff:fec4:cbce/64
## private ip 
eth1             UP             10.135.0.3/16 fe80::8081:aaff:feaa:780/64

snap install microk8s --classic 
## namensaufloesung fuer pods 
microk8s enable dns 

```

```
## Funktioniert microk8s 
microk8s status
```

### Steps 2: Server 2+3 (automatische Installation -> microk8s ) 

```
## Was macht das ? 
## 1. Basisnutzer (11trainingdo) - keine Voraussetzung für microk8s
## 2. Installation von microk8s 
##.>>>>>>> microk8s installiert <<<<<<<<
## - snap install --classic microk8s 
## >>>>>>> Zuordnung zur Gruppe microk8s - notwendig für bestimmte plugins (z.B. helm)  
## usermod -a -G microk8s root 
## >>>>>>> Setzen des .kube - Verzeichnisses auf den Nutzer microk8s -> nicht zwingend erforderlich 
## chown -r -R microk8s ~/.kube 
## >>>>>>> REQUIRED .. DNS aktivieren, wichtig für Namensauflösungen innerhalb der PODS
## >>>>>>> sonst funktioniert das nicht !!! 
## microk8s enable dns 
## >>>>>>> kubectl alias gesetzt, damit man nicht immer microk8s kubectl eingeben muss
## - echo "alias kubectl='microk8s kubectl'" >> /root/.bashrc

## cloud-init script 
## s.u. MITMICROK8S (keine Voraussetzung - nur zum Einrichten des Nutzers 11trainingdo per ssh) 
##cloud-config
users:
  - name: 11trainingdo
    shell: /bin/bash

runcmd:
  - sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
  - echo " " >> /etc/ssh/sshd_config 
  - echo "AllowUsers 11trainingdo" >> /etc/ssh/sshd_config 
  - echo "AllowUsers root" >> /etc/ssh/sshd_config 
  - systemctl reload sshd 
  - sed -i '/11trainingdo/c 11trainingdo:$6$HeLUJW3a$4xSfDFQjKWfAoGkZF3LFAxM4hgl3d6ATbr2kEu9zMOFwLxkYMO.AJF526mZONwdmsm9sg0tCBKl.SYbhS52u70:17476:0:99999:7:::' /etc/shadow
  - echo "11trainingdo ALL=(ALL) ALL" > /etc/sudoers.d/11trainingdo
  - chmod 0440 /etc/sudoers.d/11trainingdo
  
  - echo "Installing microk8s"
  - snap install --classic microk8s
  - usermod -a -G microk8s root
  - chown -f -R microk8s ~/.kube
  - microk8s enable dns 
  - echo "alias kubectl='microk8s kubectl'" >> /root/.bashrc
```
```
## Prüfen ob microk8s - wird automatisch nach Installation gestartet
## kann eine Weile dauern
microk8s status

```

### Step 3: Client - Maschine (wir sollten nicht auf control-plane oder cluster - node arbeiten

```
Weiteren Server hochgezogen. 
Vanilla + BASIS 

## Installation Ubuntu - Server 

## cloud-init script 
## s.u. BASIS (keine Voraussetzung - nur zum Einrichten des Nutzers 11trainingdo per ssh) 

## Server 1 - manuell 
## Ubuntu 20.04 LTS - Grundinstallation 

## minimal Netzwerk - öffentlichen IP 
## nichts besonderes eingerichtet - Standard Digitalocean 

## Standard vo Installation microk8s 
lo               UNKNOWN        127.0.0.1/8 ::1/128
## public ip / interne 
eth0             UP             164.92.255.232/20 10.19.0.6/16 fe80::c:66ff:fec4:cbce/64
## private ip 
eth1             UP             10.135.0.5/16 fe80::8081:aaff:feaa:780/64

```

```
##### Installation von kubectl aus dem snap
## NICHT .. keine microk8s - keine control-plane / worker-node 
## NUR Client zum Arbeiten 
snap install kubectl --classic 

##### .kube/config 
## Damit ein Zugriff auf die kube-server-api möglich
## d.h. REST-API Interface, um das Cluster verwalten.
## Hier haben uns für den ersten Control-Node entschieden
## Alternativ wäre round-robin per dns möglich 

## Mini-Schritt 1:
## Auf dem Server 1: kubeconfig ausspielen 
microk8s config > /root/kube-config 
## auf das Zielsystem gebracht (client 1) 
scp /root/kubeconfig 11trainingdo@10.135.0.5:/home/11trainingdo

## Mini-Schritt 2:
## Auf dem Client 1 (diese Maschine) kubeconfig an die richtige Stelle bringen 
## Standardmäßig der Client nach eine Konfigurationsdatei sucht in ~/.kube/config 
sudo su -
cd 
mkdir .kube 
cd .kube 
mv /home/11trainingdo/kube-config config 

## Verbindungstest gemacht
## Damit feststellen ob das funktioniert. 
kubectl cluster-info 

```

### Schritt 4: Auf allen Servern IP's hinterlegen und richtigen Hostnamen überprüfen 

```
## Auf jedem Server 
hostnamectl 
## evtl. hostname setzen 
## z.B. - auf jedem Server eindeutig 
hostnamectl set-hostname n1.training.local 

## Gleiche hosts auf allen server einrichten.
## Wichtig, um Traffic zu minimieren verwenden, die interne (private) IP

/etc/hosts 
10.135.0.3 n1.training.local n1
10.135.0.4 n2.training.local n2
10.135.0.5 n3.training.local n3 

```

### Schritt 5: Cluster aufbauen 

```
## Mini-Schritt 1:
## Server 1: connection - string (token) 
microk8s add-node 
## Zeigt Liste und wir nehmen den Eintrag mit der lokalen / öffentlichen ip
## Dieser Token kann nur 1x verwendet werden und wir auf dem ANDEREN node ausgeführt
## microk8s join 10.135.0.3:25000/e9cdaa11b5d6d24461c8643cdf107837/bcad1949221a

## Mini-Schritt 2:
## Dauert eine Weile, bis das durch ist. 
## Server 2: Den Node hinzufügen durch den JOIN - Befehl 
microk8s join 10.135.0.3:25000/e9cdaa11b5d6d24461c8643cdf107837/bcad1949221a

## Mini-Schritt 3:
## Server 1: token besorgen für node 3
microk8s add-node 

## Mini-Schritt 4:
## Server 3: Den Node hinzufügen durch den JOIN-Befehl 
microk8s join 10.135.0.3:25000/09c96e57ec12af45b2752fb45450530c/bcad1949221a

## Mini-Schritt 5: Überprüfen ob HA-Cluster läuft 
Server 1: (es kann auf jedem der 3 Server überprüft werden, auf einem reicht 
microk8s status | grep high-availability 
high-availability: yes 
```

### Ergänzend nicht notwendige Scripte 

```
## cloud-init script 
## s.u. BASIS (keine Voraussetzung - nur zum Einrichten des Nutzers 11trainingdo per ssh) 

## Digitalocean - unter user_data reingepastet beim Einrichten 

##cloud-config
users:
  - name: 11trainingdo
    shell: /bin/bash

runcmd:
  - sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
  - echo " " >> /etc/ssh/sshd_config 
  - echo "AllowUsers 11trainingdo" >> /etc/ssh/sshd_config 
  - echo "AllowUsers root" >> /etc/ssh/sshd_config 
  - systemctl reload sshd 
  - sed -i '/11trainingdo/c 11trainingdo:$6$HeLUJW3a$4xSfDFQjKWfAoGkZF3LFAxM4hgl3d6ATbr2kEu9zMOFwLxkYMO.AJF526mZONwdmsm9sg0tCBKl.SYbhS52u70:17476:0:99999:7:::' /etc/shadow
  - echo "11trainingdo ALL=(ALL) ALL" > /etc/sudoers.d/11trainingdo
  - chmod 0440 /etc/sudoers.d/11trainingdo
```

## Kubernetes - microk8s (Installation und Management) 

### Installation Ubuntu - snap


### Walkthrough

```
sudo snap install microk8s --classic
microk8s status

## Sobald Kubernetes zur Verfügung steht aktivieren wir noch das plugin dns
microk8s enable dns 
microk8s status
```

### Optional

```
## Execute kubectl commands like so 
microk8s kubectl
microk8s kubectl cluster-info

## Make it easier with an alias 
echo "alias kubectl='microk8s kubectl'" >> ~/.bashrc
source ~/.bashrc
kubectl

```
### Working with snaps 

```
snap info microk8s 

```

### Ref:

  * https://microk8s.io/docs/setting-snap-channel

### Patch to next major release - cluster

### Remote-Verbindung zu Kubernetes (microk8s) einrichten


```
## on CLIENT install kubectl 
sudo snap install kubectl --classic 

## On MASTER -server get config 
## als root
cd
microk8s config > /home/kurs/remote_config 

## Download (scp config file) and store in .kube - folder  
cd ~
mkdir .kube
cd .kube  # Wichtig: config muss nachher im verzeichnis .kube liegen 
## scp kurs@master_server:/path/to/remote_config config 
## z.B. 
scp kurs@192.168.56.102:/home/kurs/remote_config config
## oder benutzer 11trainingdo
scp 11trainingdo@192.168.56.102:/home/11trainingdo/remote_config config 

##### Evtl. IP-Adresse in config zum Server aendern 

## Ultimative 1. Test auf CLIENT 
kubectl cluster-info 

## or if using kubectl or alias 
kubectl get pods 

## if you want to use a different kube config file, you can do like so 
kubectl --kubeconfig /home/myuser/.kube/myconfig

```

### Bash completion installieren


### Walkthrough 

```
apt install bash-completion
source /usr/share/bash-completion/bash_completion
## is it installed properly 
type _init_completion

## activate for all users 
kubectl completion bash | sudo tee /etc/bash_completion.d/kubectl > /dev/null

## verifizieren - neue login shell
su -

## zum Testen
kubectl g<TAB> 
kubectl get 
```
### Alternative für k als alias für kubectl 

```
source <(kubectl completion bash)
complete -F __start_kubectl k

```

### Reference 

  * https://kubernetes.io/docs/tasks/tools/included/optional-kubectl-configs-bash-linux/

### vim support for yaml


### Ubuntu (im Unterverzeichnis /etc/vim - systemweit) 

```
hi CursorColumn cterm=NONE ctermbg=lightred ctermfg=white
autocmd FileType y?ml setlocal ts=2 sts=2 sw=2 ai number expandtab cursorline cursorcolumn
```

### Testen 

```
vim test.yml 
Eigenschaft: <return> # springt eingerückt in die nächste Zeile um 2 spaces eingerückt

## evtl funktioniert vi test.yml auf manchen Systemen nicht, weil kein vim (vi improved) 


```

### kubectl unter windows - Remote-Verbindung zu Kuberenets (microk8s) einrichten


### Walkthrough (Installation)

```
## Step 1
chocolatry installiert. 
(powershell als Administrator ausführen)
## https://docs.chocolatey.org/en-us/choco/setup
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

## Step 2
choco install kubernetes-cli 

## Step 3
testen:
kubectl version --client 

## Step 4:
## powershell als normaler benutzer öffnen 
```

### Walkthrough (autocompletion) 

```
in powershell (normaler Benutzer) 
kubectl completion powershell | Out-String | Invoke-Expression
```

### kubectl - config - Struktur vorbereiten  

```
## in powershell im heimatordner des Benutzers .kube - ordnern anlegen
## C:\Users\<dein-name>\
mkdir .kube 
cd .kube 
```

### IP von Cluster-Node bekommen 

```
## auf virtualbox - maschine per ssh einloggen 
## öffentliche ip herausfinden - z.B. enp0s8 bei HostOnly - Adapter
ip -br a 
```

### config für kubectl aus Cluster-Node auslesen (microk8s) 

```
## auf virtualbox - maschine per ssh einloggen / zum root wechseln 
## abfragen
microk8s config 

## Alle Zeilen ins clipboard kopieren
## und mit notepad++ in die Datei \Users\<dein-name>\.kube\config 
## schreiben

## Wichtig: Zeile cluster -> clusters / server 
## Hier ip von letztem Schritt eintragen:
## z.B. 
Server: https://192.168.56.106/......
```

### Testen 

```
## in powershell
## kann ich eine Verbindung zum Cluster aufbauen ? 
kubectl cluster-info 
```



  * https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/

### Create a cluster with microk8s


### Walkthrough 

```
## auf master (jeweils für jedes node neu ausführen)
microk8s add-node

## dann auf jeweiligem node vorigen Befehl der ausgegeben wurde ausführen
## Kann mehr als 60 sekunden dauern ! Geduld...Geduld..Geduld 
##z.B. -> ACHTUNG evtl. IP ändern 
microk8s join 10.128.63.86:25000/567a21bdfc9a64738ef4b3286b2b8a69

```

### Auf einem Node addon aktivieren z.B. ingress

```
gucken, ob es auf dem anderen node auch aktiv ist. 
```

### Ref:

  * https://microk8s.io/docs/high-availability

### Ingress controller in microk8s aktivieren


### Aktivieren

```
microk8s enable ingress
```

### Referenz 

  * https://microk8s.io/docs/addon-ingress

### Arbeiten mit der Registry

### Installation Kubernetes Dashboard


### Reference:

  * https://blog.tippybits.com/installing-kubernetes-in-virtualbox-3d49f666b4d6    

## Kubernetes Praxis API-Objekte 

### Das Tool kubectl (Devs/Ops) - Spickzettel


### Allgemein 

```
## Zeige Information über das Cluster 
kubectl cluster-info 

## Welche api-resources gibt es ?
kubectl api-resources 

## Hilfe zu object und eigenschaften bekommen
kubectl explain pod 
kubectl explain pod.metadata
kubectl explain pod.metadata.name 

```

### Arbeiten mit manifesten 

```
kubectl apply -f nginx-replicaset.yml 
## Wie ist aktuell die hinterlegte config im system
kubectl get -o yaml -f nginx-replicaset.yml 

## Änderung in nginx-replicaset.yml z.B. replicas: 4 
## dry-run - was wird geändert 
kubectl diff -f nginx-replicaset.yml 

## anwenden 
kubectl apply -f nginx-replicaset.yml 

## Alle Objekte aus manifest löschen
kubectl delete -f nginx-replicaset.yml 


```

### Ausgabeformate 

```
## Ausgabe kann in verschiedenen Formaten erfolgen 
kubectl get pods -o wide # weitere informationen 
## im json format
kubectl get pods -o json 

## gilt natürluch auch für andere kommandos
kubectl get deploy -o json 
kubectl get deploy -o yaml 
```



### Zu den Pods 

```
## Start einen pod // BESSER: direkt manifest verwenden
## kubectl run podname image=imagename 
kubectl run nginx image=nginx 

## Pods anzeigen 
kubectl get pods 
kubectl get pod
## Format weitere Information 
kubectl get pod -o wide 
## Zeige labels der Pods
kubectl get pods --show-labels 

## Zeige pods mit einem bestimmten label 
kubectl get pods -l app=nginx 

## Status eines Pods anzeigen 
kubectl describe pod nginx 

## Pod löschen 
kubectl delete pod nginx 

## Kommando in pod ausführen 
kubectl exec -it nginx -- bash 

```

### Arbeiten mit namespaces 

```
## Welche namespaces auf dem System 
kubectl get ns 
kubectl get namespaces 
## Standardmäßig wird immer der default namespace verwendet 
## wenn man kommandos aufruft 
kubectl get deployments 

## Möchte ich z.B. deployment vom kube-system (installation) aufrufen, 
## kann ich den namespace angeben
kubectl get deployments --namespace=kube-system 
kubectl get deployments -n kube-system 

## wir wollen unseren default namespace ändern 
kubectl config set-context --current --namespace <dein-namespace>
```



### Referenz

  * https://kubernetes.io/de/docs/reference/kubectl/cheatsheet/

### kubectl example with run


### Example (that does work)

```
## Show the pods that are running 
kubectl get pods 

## Synopsis (most simplistic example 
## kubectl run NAME --image=IMAGE_EG_FROM_DOCKER
## example
kubectl run nginx --image=nginx 

kubectl get pods 
## on which node does it run ? 
kubectl get pods -o wide 
```

### Example (that does not work) 

```
kubectl run foo2 --image=foo2
## ImageErrPull - Image konnte nicht geladen werden 
kubectl get pods 
## Weitere status - info 
kubectl describe pods foo2 
```

### Ref:

  * https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#run

### Bauen einer Applikation mit Resource Objekten


![Bauen einer Webanwendung](images/WebApp.drawio.png)

### kubectl/manifest/pod


### Walkthrough 

```
## vi nginx-static.yml 

apiVersion: v1
kind: Pod
metadata:
  name: nginx-static-web
  labels:
    webserver: nginx
spec:
  containers:
  - name: web
    image: nginx

```

```
kubectl apply -f nginx-static.yml 
kubectl describe pod nginx-static-web 
## show config 
kubectl get pod/nginx-static-web -o yaml
kubectl get pod/nginx-static-web -o wide 
```

### kubectl/manifest/replicaset


```
cd
cd manifests
mkdir 02-rs 
cd 02-rs 
vi rs.yml
```

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replica-set
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      name: template-nginx-replica-set
      labels:
        tier: frontend
    spec:
      containers:
        - name: nginx
          image: "nginx:latest"
          ports:
             - containerPort: 80
             

             
```

```
kubectl apply -f rs.yml 
```

### kubectl/manifest/deployments


```
cd
cd manifests
mkdir 03-deploy
cd 03-deploy 
nano deploy.yml 
```

```

## vi deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        
```

```
kubectl apply -f deploy.yml 
```

### Services - Aufbau


![Services Aufbau](/images/kubernetes-services.drawio.svg)

### kubectl/manifest/service


### Schritt 1: Deployment 

```
mkdir 04-service 
cd 04-service 
```

```
## 01-deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
spec:
  selector:
    matchLabels:
      run: my-nginx
  replicas: 3
  template:
    metadata:
      labels:
        run: my-nginx
    spec:
      containers:
      - name: my-nginx
        image: nginx
        ports:
        - containerPort: 80
```

```
kubectl apply -f .
```

### Schritt 2:


```
## 02-svc.yml 
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    svc: nginx
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    run: my-nginx
```

```
kubectl apply -f . 
```


### Ref.

  * https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/

### Hintergrund Ingress




### Ref. / Dokumentation 

  * https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html

### Ingress Controller auf Digitalocean (doks) mit helm installieren


### Basics 

  * Das Verfahren funktioniert auch so auf anderen Plattformen, wenn helm verwendet wird und noch kein IngressController vorhanden
  * Ist kein IngressController vorhanden, werden die Ingress-Objekte zwar angelegt, es funktioniert aber nicht. 

### Prerequisites 

  * kubectl muss eingerichtet sein 

### Walkthrough (Setup Ingress Controller) 

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm show values ingress-nginx/ingress-nginx

## It will be setup with type loadbalancer - so waiting to retrieve an ip from the external loadbalancer
## This will take a little. 
helm install nginx-ingress ingress-nginx/ingress-nginx --namespace ingress --create-namespace --set controller.publishService.enabled=true 

## See when the external ip comes available
kubectl -n ingress get all
kubectl --namespace ingress get services -o wide -w nginx-ingress-ingress-nginx-controller

## Output  
NAME                                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                      AGE     SELECTOR
nginx-ingress-ingress-nginx-controller   LoadBalancer   10.245.78.34   157.245.20.222   80:31588/TCP,443:30704/TCP   4m39s   app.kubernetes.io/component=controller,app.kubernetes.io/instance=nginx-ingress,app.kubernetes.io/name=ingress-nginx

## Now setup wildcard - domain for training purpose 
## inwx.com
*.lab1.t3isp.de A 157.245.20.222 


```


### Documentation for default ingress nginx

  * https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/

### Beispiel Ingress


### Prerequisits

```
## Ingress Controller muss aktiviert sein 
microk8s enable ingress
```

### Walkthrough 

```
mkdir abi
```


```
## apple.yml 
## vi apple.yml 
kind: Pod
apiVersion: v1
metadata:
  name: apple-app
  labels:
    app: apple
spec:
  containers:
    - name: apple-app
      image: hashicorp/http-echo
      args:
        - "-text=apple"
---

kind: Service
apiVersion: v1
metadata:
  name: apple-service
spec:
  selector:
    app: apple
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5678 # Default port for image
```

```
kubectl apply -f apple.yml 
```

```
## banana
## vi banana.yml
kind: Pod
apiVersion: v1
metadata:
  name: banana-app
  labels:
    app: banana
spec:
  containers:
    - name: banana-app
      image: hashicorp/http-echo
      args:
        - "-text=banana"

---

kind: Service
apiVersion: v1
metadata:
  name: banana-service
spec:
  selector:
    app: banana
  ports:
    - port: 80
      targetPort: 5678 # Default port for image
```

```
kubectl apply -f banana.yml
```

```
## Ingress
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
        - path: /apple
          backend:
            serviceName: apple-service
            servicePort: 80
        - path: /banana
          backend:
            serviceName: banana-service
            servicePort: 80
```

```
## ingress 
kubectl apply -f ingress.yml
kubectl get ing 
```

### Reference 

  * https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html

### Find the problem 

```
## Hints 

## 1. Which resources does our version of kubectl support 
## Can we find Ingress as "Kind" here.
kubectl api-ressources 

## 2. Let's see, how the configuration works 
kubectl explain --api-version=networking.k8s.io/v1 ingress.spec.rules.http.paths.backend.service

## now we can adjust our config 
```

### Solution

```
## in kubernetes 1.22.2 - ingress.yml needs to be modified like so.
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
        - path: /apple
          pathType: Prefix
          backend:
            service:
              name: apple-service
              port:
                number: 80
        - path: /banana
          pathType: Prefix
          backend:
            service:
              name: banana-service
              port:
                number: 80                
```

### Install Ingress On Digitalocean DOKS

### Beispiel mit Hostnamen


### Prerequisits

```
## Ingress Controller muss aktiviert sein 
### Nur der Fall wenn man microk8s zum Einrichten verwendet 
### Ubuntu 
microk8s enable ingress
```

### Walkthrough 

#### Step 1: pods and services

```
cd
mkdir -p manifests
cd manifests 
mkdir abi
cd abi
```

```
## apple.yml 
## vi apple.yml 
kind: Pod
apiVersion: v1
metadata:
  name: apple-app
  labels:
    app: apple
spec:
  containers:
    - name: apple-app
      image: hashicorp/http-echo
      args:
        - "-text=apple-<dein-name>"
---

kind: Service
apiVersion: v1
metadata:
  name: apple-service
spec:
  selector:
    app: apple
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5678 # Default port for image
```

```
kubectl apply -f apple.yml 
```

```
## banana
## vi banana.yml
kind: Pod
apiVersion: v1
metadata:
  name: banana-app
  labels:
    app: banana
spec:
  containers:
    - name: banana-app
      image: hashicorp/http-echo
      args:
        - "-text=banana-<dein-name>"

---

kind: Service
apiVersion: v1
metadata:
  name: banana-service
spec:
  selector:
    app: banana
  ports:
    - port: 80
      targetPort: 5678 # Default port for image
```

```
kubectl apply -f banana.yml
```

### Step 2: Ingress 

```
## Ingress
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
    # with the ingress controller from helm, you need to set an annotation 
    # otherwice it does not know, which controller to use
    # old version... use ingressClassName instead 
    # kubernetes.io/ingress.class: nginx
spec:
  ingressClassName: nginx
  rules:
  - host: "<euername>.lab<nr>.t3isp.de"
    http:
      paths:
        - path: /apple
          backend:
            serviceName: apple-service
            servicePort: 80
        - path: /banana
          backend:
            serviceName: banana-service
            servicePort: 80
```

```
## ingress 
kubectl apply -f ingress.yml
kubectl get ing 
```

### Reference 

  * https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html

### Find the problem 

```
## Hints 

## 1. Which resources does our version of kubectl support 
## Can we find Ingress as "Kind" here.
kubectl api-ressources 

## 2. Let's see, how the configuration works 
kubectl explain --api-version=networking.k8s.io/v1 ingress.spec.rules.http.paths.backend.service

## now we can adjust our config 
```

### Solution

```
## in kubernetes 1.22.2 - ingress.yml needs to be modified like so.
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
    # with the ingress controller from helm, you need to set an annotation 
    # old version useClassName instead 
    # otherwice it does not know, which controller to use
    # kubernetes.io/ingress.class: nginx 
spec:
  ingressClassName: nginx
  rules:
  - host: "app12.lab.t3isp.de"
    http:
      paths:
        - path: /apple
          pathType: Prefix
          backend:
            service:
              name: apple-service
              port:
                number: 80
        - path: /banana
          pathType: Prefix
          backend:
            service:
              name: banana-service
              port:
                number: 80                
```

### Achtung: Ingress mit Helm - annotations

### Permanente Weiterleitung mit Ingress


### Example

```
## redirect.yml 
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace

---

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/permanent-redirect: https://www.google.de
    nginx.ingress.kubernetes.io/permanent-redirect-code: "308"
  creationTimestamp: null
  name: destination-home
  namespace: my-namespace
spec:
  rules:
  - host: web.training.local
    http:
      paths:
      - backend:
          service:
            name: http-svc
            port:
              number: 80
        path: /source
        pathType: ImplementationSpecific
```

```
Achtung: host-eintrag auf Rechner machen, von dem aus man zugreift 

/etc/hosts 
45.23.12.12 web.training.local
```


```
curl -I  http://web.training.local/source
HTTP/1.1 308 
Permanent Redirect 

```

### Umbauen zu google ;o) 

```
This annotation allows to return a permanent redirect instead of sending data to the upstream. For example nginx.ingress.kubernetes.io/permanent-redirect: https://www.google.com would redirect everything to Google.

```

### Refs:

  * https://github.com/kubernetes/ingress-nginx/blob/main/docs/user-guide/nginx-configuration/annotations.md#permanent-redirect
  * 

### ConfigMap Example


### Schritt 1: configmap vorbereiten 
```
cd 
mkdir -p manifests 
cd manifests
mkdir configmaptests 
cd configmaptests
nano 01-configmap.yml
```

```
### 01-configmap.yml
kind: ConfigMap 
apiVersion: v1 
metadata:
  name: example-configmap 
data:
  # als Wertepaare
  database: mongodb
  database_uri: mongodb://localhost:27017
```

```
kubectl apply -f 01-configmap.yml 
kubectl get cm
kubectl get cm -o yaml
```

### Schrit 2: Beispiel als Datei 


```
nano 02-pod.yml
```

```
kind: Pod 
apiVersion: v1 
metadata:
  name: pod-mit-configmap 

spec:
  # Add the ConfigMap as a volume to the Pod
  volumes:
    # `name` here must match the name
    # specified in the volume mount
    - name: example-configmap-volume
      # Populate the volume with config map data
      configMap:
        # `name` here must match the name 
        # specified in the ConfigMap's YAML 
        name: example-configmap

  containers:
    - name: container-configmap
      image: nginx:latest
      # Mount the volume that contains the configuration data 
      # into your container filesystem
      volumeMounts:
        # `name` here must match the name
        # from the volumes section of this pod
        - name: example-configmap-volume
          mountPath: /etc/config


```

```
kubectl apply -f 02-pod.yml 
```

```
##Jetzt schauen wir uns den Container/Pod mal an
kubectl exec pod-mit-configmap -- ls -la /etc/config
kubectl exec -it pod-mit-configmap --  bash
## ls -la /etc/config 
```

### Schritt 3: Beispiel. ConfigMap als env-variablen 

```
nano 03-pod-mit-env.yml
```

```
## 03-pod-mit-env.yml 
kind: Pod 
apiVersion: v1 
metadata:
  name: pod-env-var 
spec:
  containers:
    - name: env-var-configmap
      image: nginx:latest 
      envFrom:
        - configMapRef:
            name: example-configmap

```

```
kubectl apply -f 03-pod-mit-env.yml
```

```
## und wir schauen uns das an 
##Jetzt schauen wir uns den Container/Pod mal an
kubectl exec pod-env-var -- env
kubectl exec -it pod-env-var --  bash
## env

```


### Reference: 

 * https://matthewpalmer.net/kubernetes-app-developer/articles/ultimate-configmap-guide-kubernetes.html

### Configmap MariadDB - Example


### Schritt 1: configmap 

```
### 01-configmap.yml
kind: ConfigMap 
apiVersion: v1 
metadata:
  name: mariadb-configmap 
data:
  # als Wertepaare
  MARIADB_ROOT_PASSWORD: 11abc432
```

```
kubectl apply -f .
kubectl get cm
kubectl get cm mariadb-configmap -o yaml
```


### Schritt 2: Deployment 
```
cd 
mkdir -p manifests 
cd manifests
mkdir configmapmaria
cd configmapmaria
nano 01-deploy.yml
```

```
##deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mariadb-deployment
spec:
  selector:
    matchLabels:
      app: mariadb
  replicas: 1 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: mariadb
    spec:
      containers:
      - name: mariadb-cont
        image: mariadb:latest
        envFrom:
        - configMapRef:
            name: mariadb-configmap

```

```
kubectl apply -f .
```

### Imporatnt Sidenode 

  * If configmap changes, deployment does not know
  * So kubectl apply -f deploy.yml will not have any effect
  * to fix, use stakater/relaoder: https://github.com/stakater/Reloader


## Helm (Kubernetes Paketmanager) 

### Helm Grundlagen


### Wo ? 

```
artifacts helm 

```

 * https://artifacthub.io/

### Komponenten 

```
Chart - beeinhaltet Beschreibung und Komponenten 
tar.gz - Format 
oder Verzeichnis 

Wenn wir ein Chart ausführen wird eine Release erstellen 
(parallel: image -> container, analog: chart -> release)
```

### Installation 

```
## Beispiel ubuntu 
## snap install --classic helm

## Cluster muss vorhanden, aber nicht notwendig wo helm installiert 

## Voraussetzung auf dem Client-Rechner (helm ist nichts als anderes als ein Client-Programm) 
Ein lauffähiges kubectl auf dem lokalen System (welches sich mit dem Cluster verbinden kann).
-> saubere -> .kube/config 

## Test
kubectl cluster-info 

```


### Helm Warum ?


```
Ein Paket für alle Komponenten
Einfaches Installieren, Updaten und deinstallieren 
Feststehende Struktur 
```

### Helm Example


### Prerequisites 

  * kubectl needs to be installed and configured to access cluster
  * Good: helm works as unprivileged user as well - Good for our setup 
  * install helm on ubuntu (client) as root: snap install --classic helm 
    * this installs helm3
  * Please only use: helm3. No server-side components needed (in cluster) 
    * Get away from examples using helm2 (hint: helm init) - uses tiller  

### Simple Walkthrough (Example 0)

```
## Repo hinzufpgen 
helm repo add bitnami https://charts.bitnami.com/bitnami 
## gecachte Informationen aktualieren 
helm repo update

helm search repo bitnami 
## helm install release-name bitnami/mysql
helm install my-mysql bitnami/mysql
## Chart runterziehen ohne installieren 
## helm pull bitnami/mysql

## Release anzeigen zu lassen
helm list 

## Status einer Release / Achtung, heisst nicht unbedingt nicht, dass pod läuft 
helm status my-mysql 

## weitere release installieren 
## helm install neuer-release-name  bitnami/mysql 


```

### Under the hood 

```
## Helm speichert Informationen über die Releases in den Secrets
kubectl get secrets | grep helm 


```


### Example 1: - To get know the structure 

```
helm repo add bitnami https://charts.bitnami.com/bitnami 
helm search repo bitnami 
helm repo update
helm pull bitnami/mysql 
tar xzvf mysql-9.0.0.tgz 

```



### Example 2: We will setup mysql without persistent storage (not helpful in production ;o() 

```
helm repo add bitnami https://charts.bitnami.com/bitnami 
helm search repo bitnami 
helm repo update

helm install my-mysql bitnami/mysql


```


### Example 2 - continue - fehlerbehebung 

```
helm uninstall my-mysql 
## Install with persistentStorage disabled - Setting a specific value 
helm install my-mysql --set primary.persistence.enabled=false bitnami/mysql

## just as notice 
## helm uninstall my-mysql 

```

### Example 2b: using a values file 

```
## mkdir helm-mysql
## cd helm-mysql
## vi values.yml 
primary:
  persistence:
    enabled: false 
```

```
helm uninstall my-mysql
helm install my-mysql bitnami/mysql -f values.yml 
```

### Example 3: Install wordpress 

```
helm repo add bitnami https://charts.bitnami.com/bitnami 
helm install my-wordpress \
  --set wordpressUsername=admin \
  --set wordpressPassword=password \
  --set mariadb.auth.rootPassword=secretpassword \
    bitnami/wordpress
```

### Referenced

  * https://github.com/bitnami/charts/tree/master/bitnami/mysql/#installing-the-chart
  * https://helm.sh/docs/intro/quickstart/

## Kubernetes - RBAC 

### Nutzer einrichten


### Enable RBAC in microk8s 

```
## This is important, if not enable every user on the system is allowed to do everything 
microk8s enable rbac 
```

### Schritt 1: Nutzer-Account auf Server anlegen / in Client 

```
cd 
mkdir -p manifests/rbac
cd manifests/rbac
```

####  Mini-Schritt 1: Definition für Nutzer 

```
## vi service-account.yml 
apiVersion: v1
kind: ServiceAccount
metadata:
  name: training
  namespace: default


kubectl apply -f service-account.yml 
```


#### Mini-Schritt 2: ClusterRolle festlegen - Dies gilt für alle namespaces, muss aber noch zugewiesen werden

```
### Bevor sie zugewiesen ist, funktioniert sie nicht - da sie keinem Nutzer zugewiesen ist 

## vi pods-clusterrole.yml 
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: pods-clusterrole
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

kubectl apply -f pods-clusterrole.yml 
```

#### Mini-Schritt 3: Die ClusterRolle den entsprechenden Nutzern über RoleBinding zu ordnen 
```
## vi rb-training-ns-default-pods.yml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: rolebinding-ns-default-pods
  namespace: default
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: pods-clusterrole 
subjects:
- kind: ServiceAccount
  name: training
  namespace: default

kubectl apply -f rb-training-ns-default-pods.yml

```

#### Mini-Schritt 4: Testen (klappt der Zugang) 

```
kubectl auth can-i get pods -n default --as system:serviceaccount:default:training
```

### Schritt 2: Context anlegen / Credentials auslesen und in kubeconfig hinterlegen 

#### Mini-Schritt 1: kubeconfig setzen 
```
kubectl config set-context training-ctx --cluster microk8s-cluster --user training

## extract name of the token from here 
TOKEN_NAME=`kubectl get serviceaccount training -o jsonpath='{.secrets[0].name}'`

TOKEN=`kubectl get secret $TOKEN_NAME -o jsonpath='{.data.token}' | base64 --decode`
echo $TOKEN
kubectl config set-credentials training --token=$TOKEN
kubectl config use-context training-ctx

## Hier reichen die Rechte nicht aus 
kubectl get deploy
## Error from server (Forbidden): pods is forbidden: User "system:serviceaccount:kube-system:training" cannot list # resource "pods" in API group "" in the namespace "default"
```

#### Mini-Schritt 2:
```
kubectl config use-context training-ctx
kubectl get pods 
```

### Refs:

  * https://docs.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengaddingserviceaccttoken.htm
  * https://microk8s.io/docs/multi-user
  * https://faun.pub/kubernetes-rbac-use-one-role-in-multiple-namespaces-d1d08bb08286



## Kubernetes - Netzwerk (CNI's) / Mesh

### Übersicht Netzwerke


### CNI 

  * Common Network Interface
  * Fest Definition, wie Container mit Netzwerk-Bibliotheken kommunizieren

### Docker - Container oder andere 

  * Container wird hochgefahren -> über CNI -> zieht Netzwerk - IP  hoch. 
  * Container witd runtergahren -> uber CNI -> Netzwerk - IP wird released 

### Welche gibt es ? 

  * Flanel
  * Canal 
  * Calico 
  * Cilium
  
### Flannel

#### Overlay - Netzwerk 

  * virtuelles Netzwerk was sich oben drüber und eigentlich auf Netzwerkebene nicht existiert
  * VXLAN 

#### Vorteile 

  * Guter einfacher Einstieg 
  * redziert auf eine Binary flanneld 

#### Nachteile 

  * keine Firewall - Policies möglich 
  * keine klassichen Netzwerk-Tools zum Debuggen möglich. 

### Canal 

#### General 

  * Auch ein Overlay - Netzwerk 
  * Unterstüzt auch policies 

### Calico

#### Generell 

  * klassische Netzwerk (BGP)

#### Vorteile gegenüber Flannel 

  * Policy über Kubernetes Object (NetworkPolicies)

#### Vorteile 

  * ISTIO integrierbar (Mesh - Netz) 
  * Performance etwas besser als Flannel (weil keine Encapsulation)

#### Referenz 
  * https://projectcalico.docs.tigera.io/security/calico-network-policy

### microk8s Vergleich 

  * https://microk8s.io/compare

```
snap.microk8s.daemon-flanneld
Flannel is a CNI which gives a subnet to each host for use with container runtimes.

Flanneld runs if ha-cluster is not enabled. If ha-cluster is enabled, calico is run instead.

The flannel daemon is started using the arguments in ${SNAP_DATA}/args/flanneld. For more information on the configuration, see the flannel documentation.
```

### Callico - nginx example


```
## Schritt 1:
kubectl create ns policy-demo
kubectl create deployment --namespace=policy-demo nginx --image=nginx
kubectl expose --namespace=policy-demo deployment nginx --port=80
## lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo access --rm -ti --image busybox /bin/sh
```
```
## innerhalb der shell 
wget -q nginx -O -
```
```
## Schritt 2: Policy festlegen, dass kein Ingress-Traffic erlaubt
## in diesem namespace: policy-demo 
kubectl create -f - <<EOF
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: default-deny
  namespace: policy-demo
spec:
  podSelector:
    matchLabels: {}
EOF
## lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo access --rm -ti --image busybox /bin/sh
```

```
## innerhalb der shell 
wget -q nginx -O -
```

```
## Schritt 3: Zugriff erlauben von pods mit dem Label run=access 
kubectl create -f - <<EOF
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: access-nginx
  namespace: policy-demo
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
    - from:
      - podSelector:
          matchLabels:
            run: access
EOF

## lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
## pod hat durch run -> access automatisch das label run:access zugewiesen 
kubectl run --namespace=policy-demo access --rm -ti --image busybox /bin/sh
```

```
## innerhalb der shell 
wget -q nginx -O -
```

``` 
kubectl run --namespace=policy-demo no-access --rm -ti --image busybox /bin/sh
```

```
## in der shell  
wget -q nginx -O -
```

```

kubectl delete ns policy-demo 

```


### Ref:

  * https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-basic

### Callico - client-backend-ui-example


### Walkthrough 

```
cd
mkdir -p manifests/callico/example1 
cd manifests/callico/example1 
```

```
### Step 1: Create containers 

kubectl create -f https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/manifests/00-namespace.yaml
kubectl create -f https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/manifests/01-management-ui.yaml
kubectl create -f https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/manifests/02-backend.yaml
kubectl create -f https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/manifests/03-frontend.yaml
kubectl create -f https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/manifests/04-client.yaml

kubectl get pods --all-namespaces --watch
kubectl get ns 
```

```
### Step 2: Check connections in the browser (ui) 
### Use IP of one of your nodes here 
http://164.92.255.234:30002/
```

```
### Step 3: Download default-deny rules 
wget https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/policies/default-deny.yaml
### Let us have look into it 
### Deny all pods 
cat default-deny.yaml 
### Apply this for 2 namespaces created in Step 1
kubectl -n client apply -f default-deny.yaml 
kubectl -n stars apply -f default-deny.yaml 

```

```
### Step 4: Refresh UI and see, that there are no connections possilbe 
http://164.92.255.234:30002/
```

```
### Step 5: 
### Allow traffic by policy 
wget https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/policies/allow-ui.yaml
wget https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/policies/allow-ui-client.yaml
### Let us look into this: 
cat allow-ui.yaml 
cat allow-ui-client.yaml
kubectl apply -f allow-ui.yaml
kubectl apply -f allow-ui-client.yaml 

```

```
### Step 6: 
### Refresh management ui 
### Now all traffic is allowed
http://164.92.255.234:30002/
```

```
### Step 7:
### Restrict traffic to backend 
wget https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/policies/backend-policy.yaml
cat backend-policy.yaml 
kubectl apply -f backend-policy.yaml


```

```
### Step 8:
### Refresh 
## The frontend can now access the backend (on TCP port 6379 only).
## The backend cannot access the frontend at all.
## The client cannot access the frontend, nor can it access the backend
http://164.92.255.234:30002/


```

```
### Step 9: 
wget https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/policies/frontend-policy.yaml
cat frontend-policy.yaml 
kubectl apply -f frontend-policy.yaml 
```

```
### Step 10:
## Refresh ui 
## Client can now access Frontend 
http://164.92.255.234:30002/

```

```
## Alles wieder löschen 
kubectl delete ns client stars management-ui


```



### Reference 

  * https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-demo/kubernetes-demo

### Mesh / istio


### Schaubild 

![istio Schaubild](https://istio.io/latest/docs/examples/virtual-machines/vm-bookinfo.svg)

### Istio 

```
## Visualization 
## with kiali (included in istio) 
https://istio.io/latest/docs/tasks/observability/kiali/kiali-graph.png

## Example 
## https://istio.io/latest/docs/examples/bookinfo/
The sidecars are injected in all pods within the namespace by labeling the namespace like so:
kubectl label namespace default istio-injection=enabled

## Gateway (like Ingress in vanilla Kubernetes) 
kubectl label namespace default istio-injection=enabled
```

### istio tls 

 * https://istio.io/latest/docs/ops/configuration/traffic-management/tls-configuration/


### istio - the next generation without sidecar 

  * https://istio.io/latest/blog/2022/introducing-ambient-mesh/

## kubectl 

### Start pod (container with run && examples)


### Example (that does work)

```
## Show the pods that are running 
kubectl get pods 

## Synopsis (most simplistic example 
## kubectl run NAME --image=IMAGE_EG_FROM_DOCKER
## example
kubectl run nginx --image=nginx 

kubectl get pods 
## on which node does it run ? 
kubectl get pods -o wide 
```

### Example (that does not work) 

```
kubectl run foo2 --image=foo2
## ImageErrPull - Image konnte nicht geladen werden 
kubectl get pods 
## Weitere status - info 
kubectl describe pods foo2 
```

### Ref:

  * https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#run

### Bash completion for kubectl


### Walkthrough 

```
apt install bash-completion
source /usr/share/bash-completion/bash_completion
## is it installed properly 
type _init_completion

## activate for all users 
kubectl completion bash | sudo tee /etc/bash_completion.d/kubectl > /dev/null

## verifizieren - neue login shell
su -

## zum Testen
kubectl g<TAB> 
kubectl get 
```
### Alternative für k als alias für kubectl 

```
source <(kubectl completion bash)
complete -F __start_kubectl k

```

### Reference 

  * https://kubernetes.io/docs/tasks/tools/included/optional-kubectl-configs-bash-linux/

### kubectl Spickzettel


### Allgemein 

```
## Zeige Information über das Cluster 
kubectl cluster-info 

## Welche api-resources gibt es ?
kubectl api-resources 

## Hilfe zu object und eigenschaften bekommen
kubectl explain pod 
kubectl explain pod.metadata
kubectl explain pod.metadata.name 

```

### Arbeiten mit manifesten 

```
kubectl apply -f nginx-replicaset.yml 
## Wie ist aktuell die hinterlegte config im system
kubectl get -o yaml -f nginx-replicaset.yml 

## Änderung in nginx-replicaset.yml z.B. replicas: 4 
## dry-run - was wird geändert 
kubectl diff -f nginx-replicaset.yml 

## anwenden 
kubectl apply -f nginx-replicaset.yml 

## Alle Objekte aus manifest löschen
kubectl delete -f nginx-replicaset.yml 


```

### Ausgabeformate 

```
## Ausgabe kann in verschiedenen Formaten erfolgen 
kubectl get pods -o wide # weitere informationen 
## im json format
kubectl get pods -o json 

## gilt natürluch auch für andere kommandos
kubectl get deploy -o json 
kubectl get deploy -o yaml 
```



### Zu den Pods 

```
## Start einen pod // BESSER: direkt manifest verwenden
## kubectl run podname image=imagename 
kubectl run nginx image=nginx 

## Pods anzeigen 
kubectl get pods 
kubectl get pod
## Format weitere Information 
kubectl get pod -o wide 
## Zeige labels der Pods
kubectl get pods --show-labels 

## Zeige pods mit einem bestimmten label 
kubectl get pods -l app=nginx 

## Status eines Pods anzeigen 
kubectl describe pod nginx 

## Pod löschen 
kubectl delete pod nginx 

## Kommando in pod ausführen 
kubectl exec -it nginx -- bash 

```

### Arbeiten mit namespaces 

```
## Welche namespaces auf dem System 
kubectl get ns 
kubectl get namespaces 
## Standardmäßig wird immer der default namespace verwendet 
## wenn man kommandos aufruft 
kubectl get deployments 

## Möchte ich z.B. deployment vom kube-system (installation) aufrufen, 
## kann ich den namespace angeben
kubectl get deployments --namespace=kube-system 
kubectl get deployments -n kube-system 

## wir wollen unseren default namespace ändern 
kubectl config set-context --current --namespace <dein-namespace>
```



### Referenz

  * https://kubernetes.io/de/docs/reference/kubectl/cheatsheet/

### Tipps&Tricks zu Deploymnent - Rollout


### Warum 

```
Rückgängig machen von deploys, Deploys neu unstossen.
(Das sind die wichtigsten Fähigkeiten

```

### Beispiele 

```
## Deployment nochmal durchführen 
## z.B. nach kubectl uncordon n12.training.local 
kubectl rollout restart deploy nginx-deployment 

## Rollout rückgängig machen 
kubectl rollout undo deploy nginx-deployment 

```

## kubectl - manifest - examples 

### 02 Pod nginx mit Port und IP innerhalb des Clusters


### What is containerPort (from kubectl explain) ?

```
containerPort        <integer> -required-
     Number of port to expose on the pod's IP address. This must be a valid port
     number, 0 < x < 65536.
     
```

### Walkthrough 

```
vi nginx-static-expose.yml 

apiVersion: v1
kind: Pod
metadata:
  name: nginx-static-web
  labels:
    webserver: nginx
spec:
  containers:
  - name: web
    image: nginx
    ports:
    - name: web
      containerPort: 80
      protocol: TCP

```

```
kubectl apply -f nginx-static-expose.yml 
kubectl describe nginx-static-web 
## show config 
kubectl get pod/nginx-static-web -o yml 
```

### 03b Example with service and nginx


### Schritt 1: Deployment 

```
mkdir 04-service 
cd 04-service 
```

```
## 01-deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
spec:
  selector:
    matchLabels:
      run: my-nginx
  replicas: 3
  template:
    metadata:
      labels:
        run: my-nginx
    spec:
      containers:
      - name: my-nginx
        image: nginx
        ports:
        - containerPort: 80
```

```
kubectl apply -f .
```

### Schritt 2:


```
## 02-svc.yml 
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    svc: nginx
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    run: my-nginx
```

```
kubectl apply -f . 
```


### Ref.

  * https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/

### 04 Ingress mit einfachem Beispiel


### Prerequisits

```
## Ingress Controller muss aktiviert sein 
microk8s enable ingress
```

### Walkthrough 

```
mkdir abi
```


```
## apple.yml 
## vi apple.yml 
kind: Pod
apiVersion: v1
metadata:
  name: apple-app
  labels:
    app: apple
spec:
  containers:
    - name: apple-app
      image: hashicorp/http-echo
      args:
        - "-text=apple"
---

kind: Service
apiVersion: v1
metadata:
  name: apple-service
spec:
  selector:
    app: apple
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5678 # Default port for image
```

```
kubectl apply -f apple.yml 
```

```
## banana
## vi banana.yml
kind: Pod
apiVersion: v1
metadata:
  name: banana-app
  labels:
    app: banana
spec:
  containers:
    - name: banana-app
      image: hashicorp/http-echo
      args:
        - "-text=banana"

---

kind: Service
apiVersion: v1
metadata:
  name: banana-service
spec:
  selector:
    app: banana
  ports:
    - port: 80
      targetPort: 5678 # Default port for image
```

```
kubectl apply -f banana.yml
```

```
## Ingress
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
        - path: /apple
          backend:
            serviceName: apple-service
            servicePort: 80
        - path: /banana
          backend:
            serviceName: banana-service
            servicePort: 80
```

```
## ingress 
kubectl apply -f ingress.yml
kubectl get ing 
```

### Reference 

  * https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html

### Find the problem 

```
## Hints 

## 1. Which resources does our version of kubectl support 
## Can we find Ingress as "Kind" here.
kubectl api-ressources 

## 2. Let's see, how the configuration works 
kubectl explain --api-version=networking.k8s.io/v1 ingress.spec.rules.http.paths.backend.service

## now we can adjust our config 
```

### Solution

```
## in kubernetes 1.22.2 - ingress.yml needs to be modified like so.
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
        - path: /apple
          pathType: Prefix
          backend:
            service:
              name: apple-service
              port:
                number: 80
        - path: /banana
          pathType: Prefix
          backend:
            service:
              name: banana-service
              port:
                number: 80                
```

### 05 Ingress mit Permanent Redirect


### Example

```
## redirect.yml 
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace

---

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/permanent-redirect: https://www.google.de
    nginx.ingress.kubernetes.io/permanent-redirect-code: "308"
  creationTimestamp: null
  name: destination-home
  namespace: my-namespace
spec:
  rules:
  - host: web.training.local
    http:
      paths:
      - backend:
          service:
            name: http-svc
            port:
              number: 80
        path: /source
        pathType: ImplementationSpecific
```

```
Achtung: host-eintrag auf Rechner machen, von dem aus man zugreift 

/etc/hosts 
45.23.12.12 web.training.local
```


```
curl -I  http://web.training.local/source
HTTP/1.1 308 
Permanent Redirect 

```

### Umbauen zu google ;o) 

```
This annotation allows to return a permanent redirect instead of sending data to the upstream. For example nginx.ingress.kubernetes.io/permanent-redirect: https://www.google.com would redirect everything to Google.

```

### Refs:

  * https://github.com/kubernetes/ingress-nginx/blob/main/docs/user-guide/nginx-configuration/annotations.md#permanent-redirect
  * 

## Kubernetes - Monitoring (microk8s und vanilla) 

### metrics-server aktivieren (microk8s und vanilla)


### Warum ? Was macht er ? 

```
Der Metrics-Server sammelt Informationen von den einzelnen Nodes und Pods
Er bietet mit 

kubectl top pods
kubectl top nodes 

ein einfaches Interface, um einen ersten Eindruck über die Auslastung zu bekommen. 
```

### Walktrough 

```
## Auf einem der Nodes im Cluster (HA-Cluster) 
microk8s enable metrics-server 

## Es dauert jetzt einen Moment bis dieser aktiv ist auch nach der Installation 
## Auf dem Client
kubectl top nodes 
kubectl top pods 

```

### Kubernetes 

  * https://kubernetes-sigs.github.io/metrics-server/
  * kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

## Kubernetes - Shared Volumes 

### Shared Volumes with nfs


### Create new server and install nfs-server

```
## on Ubuntu 20.04LTS
apt install nfs-kernel-server 
systemctl status nfs-server 

vi /etc/exports 
## adjust ip's of kubernetes master and nodes 
## kmaster
/var/nfs/ 192.168.56.101(rw,sync,no_root_squash,no_subtree_check)
## knode1
/var/nfs/ 192.168.56.103(rw,sync,no_root_squash,no_subtree_check)
## knode 2
/var/nfs/ 192.168.56.105(rw,sync,no_root_squash,no_subtree_check)

exportfs -av 
```

### On all nodes (needed for production) 

```
## 
apt install nfs-common 

```

### On all nodes (only for testing)

```
#### Please do this on all servers (if you have access by ssh)
### find out, if connection to nfs works ! 

## for testing 
mkdir /mnt/nfs 
## 192.168.56.106 is our nfs-server 
mount -t nfs 192.168.56.106:/var/nfs /mnt/nfs 
ls -la /mnt/nfs
umount /mnt/nfs
```

### Persistent Storage-Step 1: Setup PersistentVolume in cluster

```
cd
cd manifests 
mkdir -p nfs 
cd nfs 
nano 01-pv.yml 
```

```
apiVersion: v1
kind: PersistentVolume
metadata:
  # any PV name
  name: pv-nfs-tln<nr>
  labels:
    volume: nfs-data-volume-tln<nr>
spec:
  capacity:
    # storage size
    storage: 1Gi
  accessModes:
    # ReadWriteMany(RW from multi nodes), ReadWriteOnce(RW from a node), ReadOnlyMany(R from multi nodes)
    - ReadWriteMany
  persistentVolumeReclaimPolicy:
    # retain even if pods terminate
    Retain
  nfs:
    # NFS server's definition
    path: /var/nfs/tln<nr>/nginx
    server: 10.135.0.8
    readOnly: false
  storageClassName: ""
```

```
kubectl apply -f 01-pv.yml 
kubectl get pv 
```

### Persistent Storage-Step 2: Create Persistent Volume Claim 

```
nano 02-pvs.yml
```

```
## vi 02-pvs.yml 
## now we want to claim space
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv-nfs-claim-tln<nr>
spec:
  storageClassName: ""
  volumeName: pv-nfs-tln<nr>
  accessModes:
  - ReadWriteMany
  resources:
     requests:
       storage: 1Gi
```


```
kubectl apply -f 02-pvs.yml
kubectl get pvc
```

### Persistent Storage-Step 3: Deployment 

```
## deployment including mount 
## vi 03-deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 4 # tells deployment to run 4 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
       
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        
        volumeMounts:
          - name: nfsvol
            mountPath: "/usr/share/nginx/html"

      volumes:
      - name: nfsvol
        persistentVolumeClaim:
          claimName: pv-nfs-claim-tln<tln>


```

```
kubectl apply -f 03-deploy.yml 

```

### Persistent Storage Step 4: service 

```
## now testing it with a service 
## cat 04-service.yml 
apiVersion: v1
kind: Service
metadata:
  name: service-nginx
  labels:
    run: svc-my-nginx
spec:
  type: NodePort
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: nginx
```        

```
kubectl apply -f 04-service.yml 
```

### Persistent Storage Step 5: write data and test

```
## connect to the container and add index.html - data 
kubectl exec -it deploy/nginx-deployment -- bash 
## in container
echo "hello dear friend" > /usr/share/nginx/html/index.html 
exit 

## now try to connect 
kubectl get svc 

## connect with ip and port
kubectl run -it --rm curly --image=curlimages/curl -- /bin/sh 
## curl http://<cluster-ip>
## exit

## now destroy deployment 
kubectl delete -f 03-deploy.yml 

## Try again - no connection 
kubectl run -it --rm curly --image=curlimages/curl -- /bin/sh 
## curl http://<cluster-ip>
## exit 
```

### Persistent Storage Step 6: retest after redeployment 

```
## now start deployment again 
kubectl apply -f 03-deploy.yml 

## and try connection again  
kubectl run -it --rm curly --image=curlimages/curl -- /bin/sh 
## curl http://<cluster-ip>
## exit 
```




## Kubernetes - Backups 

## Kubernetes - Wartung / Debugging 

### kubectl drain/uncordon


```
## Achtung, bitte keine pods verwenden, dies können "ge"-drained (ausgetrocknet) werden 
kubectl drain <node-name>
z.B. 
## Daemonsets ignorieren, da diese nicht gelöscht werden 
kubectl drain n17 --ignore-daemonsets 

## Alle pods von replicasets werden jetzt auf andere nodes verschoben 
## Ich kann jetzt wartungsarbeiten durchführen 

## Wenn fertig bin:
kubectl uncordon n17 

## Achtung: deployments werden nicht neu ausgerollt, dass muss ich anstossen.
## z.B. 
kubectl rollout restart deploy/webserver 


```

### Alte manifeste konvertieren mit convert plugin


### What is about? 

  * Plugins needs to be installed seperately on Client (or where you have your manifests) 

### Walkthrough 

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert"
## Validate the checksum
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert.sha256"
echo "$(<kubectl-convert.sha256) kubectl-convert" | sha256sum --check
## install 
sudo install -o root -g root -m 0755 kubectl-convert /usr/local/bin/kubectl-convert

## Does it work 
kubectl convert --help 

## Works like so 
## Convert to the newest version 
## kubectl convert -f pod.yaml

```

### Reference 

  * https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-convert-plugin 

### Netzwerkverbindung zu pod testen


### Situation 

```
Managed Cluster und ich kann nicht auf einzelne Nodes per ssh zugreifen
```

### Behelf: Eigenen Pod starten mit busybox 

```
kubectl run podtest --rm -ti --image busybox -- /bin/sh
```

### Example test connection 

```
## wget befehl zum Kopieren
wget -O - http://10.244.0.99
```

```
## -O -> Output (grosses O (buchstabe)) 
kubectl run podtest --rm -ti --image busybox -- /bin/sh
/ # wget -O - http://10.244.0.99
/ # exit 
```

## Kubernetes - Tipps & Tricks 

### Assigning Pods to Nodes


### Walkthrough 

```
## leave n3 as is 
kubectl label nodes n7 rechenzentrum=rz1
kubectl label nodes n17 rechenzentrum=rz2
kubectl label nodes n27 rechenzentrum=rz2

kubectl get nodes --show-labels
```

```
## nginx-deployment 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 9 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
      nodeSelector:
        rechenzentrum: rz2

## Let's rewrite that to deployment 
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    rechenzentrum=rz2



```

### Ref:

  * https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/

### Kubernetes Debuggen ClusterIP/PodIP


### Situation 

  * Kein Zugriff auf die Nodes, zum Testen von Verbindungen zu Pods und Services über die PodIP/ClusterIP 

### Lösung 

```
## Wir starten eine Busybox und fragen per wget und port ab
## busytester ist der name 
## long version 
kubectl run -it --rm --image=busybox busytester 
## wget <pod-ip-des-ziels> 
## exit 


## quick and dirty 
kubectl run -it --rm --image=busybox busytester -- wget <pod-ip-des-ziels>  

```

### Debugging pods


### How ?

  1. Which pod is in charge 
  1. Problems when starting: kubectl describe po mypod 
  1. Problems while running: kubectl logs mypod 

### Autoscaling Pods/Deployments


### Example: 

```

apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
    name: busybox-1
spec:
    scaleTargetRef:
        kind: Deployment
        name: busybox-1
    minReplicas: 3
    maxReplicas: 4
    targetCPUUtilizationPercentage: 80


```


### Reference 

  * https://medium.com/expedia-group-tech/autoscaling-in-kubernetes-why-doesnt-the-horizontal-pod-autoscaler-work-for-me-5f0094694054

## Kubernetes - Documentation 

### Documentation zu microk8s plugins/addons

  * https://microk8s.io/docs/addons

### LDAP-Anbindung

  * https://github.com/apprenda-kismatic/kubernetes-ldap

### Shared Volumes - Welche gibt es ?

  * https://kubernetes.io/docs/concepts/storage/volumes/

### Helpful to learn - Kubernetes

  * https://kubernetes.io/docs/tasks/

### Environment to learn

  * https://killercoda.com/killer-shell-cks

### Environment to learn II

  * https://killercoda.com/

### Youtube Channel

  * https://www.youtube.com/watch?v=01qcYSck1c4

## Kubernetes -Wann / Wann nicht 

### Kubernetes Wann / Wann nicht


### Frage: Kubernetes: Sollen wir das machen und was kost' mich das ? 

#### Rechtliche Regulatorien 

#### Nationale Grenzen 

#### Cloud oder onPrem (private Cloud)

#### Gegenfragen:

```
1. Monolithisches System (SAP Rx) <-> oder stark modulares System (Web-Applikation mit microservices)
    
   Kubernetes : weniger sinnvoll  <-> sehr sinnvoll.
   
   
```

#### Kosten: 

```
  o Konzeption / Planung 
  o Cluster / Manpower (Cluster-Kompetenz) 
  o Neue Backup-Strategie / Software 
  o Monitoring (ELK / EFK - STack (Elastich Search / Logstash-Fluent)) 
```

#### Anforderungen an Last 

  * Statisch (immer gleich) 
  * Dynamisch (stark wechselnd) - Einsparpotential durch Features Cloudanbieter (nur so viel bezahlen wie ich nutze) 

#### Nutzt mir Skailierung und kann ich skalieren

  * Gibt meine Applikation 
  * Habe durch mehr Webservice der gleichen Typs eine bessere Performance 

#### Kubernetes -> Kategorien. Warum ? 

  * Kosten durch Umstellung auf Cloud senken ? 
  * Automatisches Skalieren meiner Software bei Hochlast / Bedarf (verbunden mit dynamische Kosten) 
  * Erleichtertes Handling Updates (schnelleres Time-To-Market -> neuere Versioninierung) 

## Kubernetes - Hardening 

### Kubernetes Tipps Hardening


### PSA (Pod Security Admission) 

```
Policies defined by namespace.
e.g. not allowed to run container as root.

Will complain/deny when creating such a pod with that container type

```

### Möglichkeiten in Pods und Containern 

```
## für die Pods
kubectl explain pod.spec.securityContext 
kubectl explain pod.spec.containers.securityContext
```

### Example (seccomp / security context) 

```
A. seccomp - profile
https://github.com/docker/docker/blob/master/profiles/seccomp/default.json

```

```
apiVersion: v1
kind: Pod
metadata:
  name: audit-pod
  labels:
    app: audit-pod
spec:
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json

  containers:

  - name: test-container
    image: hashicorp/http-echo:0.2.3
    args:
    - "-text=just made some syscalls!"
    securityContext:
      allowPrivilegeEscalation: false

```

### SecurityContext (auf Pod Ebene) 

```
kubectl explain pod.spec.containers.securityContext 

```


### NetworkPolicy 

```
## Firewall Kubernetes 
```

## Kubernetes Deployment Scenarios 

### Deployment green/blue,canary,rolling update


### Canary Deployment 

```
A small group of the user base will see the new application 
(e.g. 1000 out of 100.000), all the others will still see the old version

From: a canary was used to test if the air was good in the mine 
(like a test balloon) 
```

### Blue / Green Deployment 

```
The current version is the Blue one 
The new version is the Green one 

New Version (GREEN) will be tested and if it works  
the traffic will be switch completey to the new version (GREEN) 

Old version can either be deleted or will function as fallback 
```

### A/B Deployment/Testing 

```
2 Different versions are online, e.g. to test a new design / new feature 
You can configure the weight (how much traffic to one or the other) 
by the number of pods
```

#### Example Calculation 

```
e.g. Deployment1: 10 pods
Deployment2: 5 pods

Both have a common label,
The service will access them through this label 
```

### Praxis-Übung A/B Deployment


### Walkthrough 

```
cd
cd manifests
mkdir ab 
cd ab 
```

```
## vi 01-cm-version1.yml 
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-version-1
data:
  index.html: |
    <html>
    <h1>Welcome to Version 1</h1>
    </br>
    <h1>Hi! This is a configmap Index file Version 1 </h1>
    </html>
```

```
## vi 02-deployment-v1.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy-v1
spec:
  selector:
    matchLabels:
      version: v1
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
        version: v1
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
            - name: nginx-index-file
              mountPath: /usr/share/nginx/html/
      volumes:
      - name: nginx-index-file
        configMap:
          name: nginx-version-1
```

```
## vi 03-cm-version2.yml 
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-version-2
data:
  index.html: |
    <html>
    <h1>Welcome to Version 2</h1>
    </br>
    <h1>Hi! This is a configmap Index file Version 2 </h1>
    </html>
```

```
## vi 04-deployment-v2.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy-v2
spec:
  selector:
    matchLabels:
      version: v2
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
        version: v2
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
            - name: nginx-index-file
              mountPath: /usr/share/nginx/html/
      volumes:
      - name: nginx-index-file
        configMap:
          name: nginx-version-2
```

```
## vi 05-svc.yml 
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    svc: nginx
spec:
  type: NodePort
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: nginx
```

```
kubectl apply -f . 
## get external ip  
kubectl get nodes -o wide 
## get port
kubectl get svc my-nginx -o wide 
## test it with curl apply it multiple time (at least ten times)
curl <external-ip>:<node-port>
```

## Kubernetes Probes (Liveness and Readiness) 

### Übung Liveness-Probe



### Übung 1: Liveness (command) 

```
What does it do ? 
 
* At the beginning pod is ready (first 30 seconds)
* Check will be done after 5 seconds of pod being startet
* Check will be done periodically every 5 minutes and will check
  * for /tmp/healthy
  * if file is there will return: 0 
  * if file is not there will return: 1 
* After 30 seconds container will be killed
* After 35 seconds container will be restarted
```

```
## cd
## mkdir -p manifests/probes
## cd manifests/probes 
## vi 01-pod-liveness-command.yml 

apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - name: liveness
    image: busybox
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -f /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```

```
## apply and test 
kubectl apply -f 01-pod-liveness-command.yml 
kubectl describe -l test=liveness pods 
sleep 30
kubectl describe -l test=liveness pods 
sleep 5 
kubectl describe -l test=liveness pods 
```

```
## cleanup
kubectl delete -f 01-pod-liveness-command.yml
 
``` 

### Übung 2: Liveness Probe (HTTP)

```
## Step 0: Understanding Prerequisite:
This is how this image works:
## after 10 seconds it returns code 500 
http.HandleFunc("/healthz", func(w http.ResponseWriter, r *http.Request) {
    duration := time.Now().Sub(started)
    if duration.Seconds() > 10 {
        w.WriteHeader(500)
        w.Write([](fmt.Sprintf("error: %v", duration.Seconds())))
    } else {
        w.WriteHeader(200)
        w.Write([]("ok"))
    }
})
```

```
## Step 1: Pod  - manifest 
## vi 02-pod-liveness-http.yml
## status-code >=200 and < 400 o.k. 
## else failure 
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-http
spec:
  containers:
  - name: liveness
    image: k8s.gcr.io/liveness
    args:
    - /server
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
        - name: Custom-Header
          value: Awesome
      initialDelaySeconds: 3
      periodSeconds: 3
```

```
## Step 2: apply and test
kubectl apply -f 02-pod-liveness-http.yml
## after 10 seconds port should have been started 
sleep 10 
kubectl describe pod liveness-http

```


### Reference:
 
   * https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/

### Funktionsweise Readiness-Probe vs. Liveness-Probe


### Why / Howto / 

  * Readiness checks, if container is ready and if it's not READY 
    * SENDS NO TRAFFIC to the container   
  

### Difference to LiveNess 

  * They are configured exactly the same, but use another keyword
    * readinessProbe instead of livenessProbe 

### Example 

```
readinessProbe:
  exec:
    command:
    - cat
    - /tmp/healthy
  initialDelaySeconds: 5
  periodSeconds: 5
```

### Reference 

  * https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-readiness-probes

## Kubernetes Interna 

### OCI,Container,Images Standards


### Schritt 1:

```
cd
mkdir bautest
cd bautest 
```

### Schritt 2:

```
## nano docker-compose.yml
version: "3.8"

services:
  myubuntu:
    build: ./myubuntu
    restart: always
```

### Schritt 3:

```
mkdir myubuntu 
cd myubuntu 
```

```
nano hello.sh
```

```
##!/bin/bash
let i=0

while true
do
  let i=i+1
  echo $i:hello-docker
  sleep 5
done

```

```
## nano Dockerfile 
FROM ubuntu:latest
RUN apt-get update; apt-get install -y inetutils-ping
COPY hello.sh .
RUN chmod u+x hello.sh
CMD ["/hello.sh"]

```

### Schritt 4: 


```
cd ../
## wichtig, im docker-compose - Ordner seiend 
##pwd 
##~/bautest
docker-compose up -d 
## wird image gebaut und container gestartet 

## Bei Veränderung vom Dockerfile, muss man den Parameter --build mitangeben 
docker-compose up -d --build 
```

## Linux und Docker Tipps & Tricks allgemein 

### Auf ubuntu root-benutzer werden


```
## kurs> 
sudo su -
## password von kurs eingegeben
## wenn wir vorher der benutzer kurs waren
```

### IP - Adresse abfragen


```
## IP-Adresse abfragen
ip a
```

### Hostname setzen


```
## als root 
hostnamectl set-hostname server.training.local 
## damit ist auch sichtbar im prompt 
su - 
```

### Proxy für Docker setzen


### Walktrough

```
## as root
systemctl list-units -t service | grep docker
systemctl cat snap.docker.dockerd.service
systemctl edit snap.docker.dockerd.service
## in edit folgendes reinschreiben
[Service]
Environment="HTTP_PROXY=http://user01:password@10.10.10.10:8080/"
Environment="HTTPS_PROXY=https://user01:password@10.10.10.10:8080/"
Environment="NO_PROXY= hostname.example.com,172.10.10.10"

systemctl show snap.docker.dockerd.service --property Environment
systemctl restart snap.docker.dockerd.service
systemctl cat snap.docker.dockerd.service
cd /etc/systemd/system/snap.docker.dockerd.service.d/
ls -la
cat override.conf
```

### Ref

  * https://www.thegeekdiary.com/how-to-configure-docker-to-use-proxy/

### vim einrückung für yaml-dateien


### Ubuntu (im Unterverzeichnis /etc/vim - systemweit) 

```
hi CursorColumn cterm=NONE ctermbg=lightred ctermfg=white
autocmd FileType y?ml setlocal ts=2 sts=2 sw=2 ai number expandtab cursorline cursorcolumn
```

### Testen 

```
vim test.yml 
Eigenschaft: <return> # springt eingerückt in die nächste Zeile um 2 spaces eingerückt

## evtl funktioniert vi test.yml auf manchen Systemen nicht, weil kein vim (vi improved) 


```

### YAML Linter Online

  * http://www.yamllint.com/

### Läuft der ssh-server


```
systemctl status sshd 
systemctl status ssh
```

### Basis/Parent - Image erstellen


### Auf Basis von debootstrap 

```
## Auf einem Debian oder Ubuntu - System 
## folgende Schritte ausführen 
## z.B. virtualbox -> Ubuntu 20.04. 

### alles mit root durchführen
apt install debootstrap
cd
debootstrap focal focal > /dev/null
tar -C focal -c . | docker import - focal 

## er gibt eine checksumme des images 
## so kann ich das sehen
## müsste focal:latest heissen
docker images

## teilchen starten 
docker run --name my_focal2 -dit focal:latest bash 

## Dann kann ich danach reinwechseln 
docker exec -it my_focal2 bash 
```

### Virtuelle Maschine Windows/OSX mit Vagrant erstellen

```
## Installieren.
https://vagrantup.com 
## ins terminal 
cd 
cd Documents 
mkdir ubuntu_20_04_test 
cd ubuntu_20_04_test
vagrant init ubuntu/focal64
vagrant up 
## Wenn die Maschine oben ist, kann direkt reinwechseln
vagrant ssh 
## in der Maschine kein pass notwendig zum Wechseln 
sudo su -

## wenn ich raus will
exit
exit

## Danach kann ich die maschine wieder zerstören
vagrant destroy -f 
```


### Ref:

  * https://docs.docker.com/develop/develop-images/baseimages/

### Eigenes unsichere Registry-Verwenden. ohne https


### Setup insecure registry (snap)

```

systemctl restart 

```

### Spiegel - Server (mirror -> registry-mirror) 

```
https://docs.docker.com/registry/recipes/mirror/

```

### Ref:

  * https://docs.docker.com/registry/insecure/

## VirtualBox Tipps & Tricks 

### VirtualBox 6.1. - Ubuntu für Kubernetes aufsetzen 


### Vorbereitung 

  * Ubuntu Server 22.04 LTS - ISO herunterladen

### Schritt 1: Virtuelle Maschine erstellen 

```
In VirtualBox Manager -> Menu -> Maschine -> Neu (Oder Neu icon) 

Seite 1:
Bei Name Ubuntu Server eingeben (dadurch wird gleich das richtige ausgewählt, bei den Selects) 
Alles andere so lassen.
Weiter 

Seite 2: 
Hauptspeicher mindest 4 GB , d.h. 4096 auswählen (füpr Kubernetes / microk8s)
Weiter

Seite 3:
Festplatte erzeugen ausgewählt lassen
Weiter

Seite 4: 
Dateityp der Festplatte: VDI ausgewählt lassen
Weiter 

Seite 5:
Art der Speicherung -> dynamisch alloziert ausgewählt lassen
Weiter 

Seite 6:
Dateiname und Größe -> bei Größe mindestens 30 GB einstellen (bei Bedarf größer) 
-> Erzeugen 
```

### Schritt 2: ISO einhängen / Netzwerk und starten / installieren

```
Neuen Server anklicken und ändern klicken:

1. 
Massenspeicher -> Controller IDE -> CD (Leer) klicken
CD - Symbol rechts neben -> Optisches Laufwerk (sekundärer Master) -> klicken -> Abbild auswählen
Downgeloadetes ISO Ubuntu 22.04 auswählen -> Öffnen klicken 

2. 
Netzwerk -> Adapter 2 (Reiter) anklicken -> Netzwerkadapter aktivieren
Angeschlossen an -> Host-only - Adapter 

3. 
unten rechts -> ok klicken 

```

### Schritt 3: Starten klicken und damit Installationsprozess beginnen

```
Try or install Ubuntu Server -> ausgewählt lassen

Seite 1:
Use up -.... Select your language 
-> English lassen
Enter eingeben

Seite 2: Keyboard Configuration
Layout auswählen (durch Navigieren mit Tab-Taste) -> Return
German auswählen (Pfeiltaste nach unten bis German, dann return)
Identify Keyboard -> Return
Keyboard Detection starting -> Ok 
Jetzt die gewünschten tasten drücken und Fragen beantworten
Layout - Variante bestätigen mit OK 

-> Done 

Seite 3: Choose type of install 
Ubuntu - Server ausgewählt lassen

-> Done 

Seite 4: Erkennung der Netzwerkkarten 
(192.168.56.1x) sollte auftauchen

-> Done 

Seite 5: Proxy

leer lassen

-> Done 

Seite 6: Mirror Address

kann so bleiben

-> Done 

Seite 7: 

Guided Storage konfiguration
Entire Disk 

-> Done 

Seite 8: File System Summary

-> Done 

Seite 9: Popup: Confirm destructive action 
Bestätigen, dass gesamte Festplatte überschrieben wird
(kein Problem, da Festplatte ohnehin leer und virtuell)

-> Continue 

Seite 10: Profile Setup

User eingeben / einrichten 
Servernamen einrichten 

-> Done 

Seite 11: SSH Setup 

Haken bei: Install OpenSSH Server 
setzen

-> Done 

Seite 12: Featured Server Snaps 

Hier brauchen wir nichts auswählen, alles kann später installiert werden

-> Done 

Seit 13:  Installation

Warten bis Installation Complete und dies auch unten angezeigt wird (Reboot Now):
(es dauert hier etwas bis alle Updates (unattended-upgrades) im Hintergrund durchgeführt worden sind) 

-> Reboot Now 

Wenn "Failed unmounting /cdrom" kommt 
dann einfach Server stoppen
-> Virtual Box Manager -> Virtuelle Maschine auswählen -> Rechte Maustaste -> Schliessen ->  Ausschalten 

```

### Schritt 4: Starten des Gast-Systems in virtualbox 

```
* Im VirtualBox Manager auf virtuelle Maschine klicken
* Neben dem Start - Pfeil -> Dreieck anklicken und Ohne Gui starten wählen 
* System startet dann im Hintergrund (kein 2. Fenster)
```

### Erklärung 

 * Console wird nicht benötigt, da wir mit putty (ssh) arbeiten zum Administrieren des Clusters
 * Putty-Verbindung muss nur auf sein, wenn wir administrieren
 * Verwendung des Clusters (nutzer/Entwickler) erfolgt ausschliesslich über kubectl in powershell !! 

### VirtualBox 6.1. - Shared folder aktivieren


### Prepare 

```
Walkthrough

## At the top menu of the virtual machine 
## Menu -> Geräte -> Gasterweiterung einlegen 

## In the console do a 
mount /dev/cdrom /mnt
cd /mnt
sudo apt-get install -y build-essential linux-headers-`uname -r`
sudo ./VBoxLinuxAdditions.run 


sudo reboot

```

### Configure 

```
Geräte -> Gemeinsame Ordner 
Hinzufügen (blaues Ordnersymbol mit + ) -> 
Ordner-Pfad: C:\Linux (Ordner muss auf Windows angelegt sein) 
Ordner-Name: linux
checkbox nicht ausgewählt bei : automatisch einbinden, nur lesbar
checkbox ausgewählt bei: Permanent erzeugen

Dann rebooten

In der virtuellen Maschine:  
sudo su -
mkdir /linux
## linux ist der vergebene Ordnername 
mount -t vboxsf linux /linux 

## Optional, falls du nicht zugreifen kannst:
sudo usermod -aG vboxsf root 
sudo usermod -aG vboxsf <your-user>


```


### persistent setzen (beim booten mounten) 
```
echo "linux	/linux	vboxsf	defaults	0	0" >> /etc/fstab 
reboot
```

### Reference:

  * https://gist.github.com/estorgio/1d679f962e8209f8a9232f7593683265
