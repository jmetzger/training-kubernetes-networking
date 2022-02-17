# Kubernetes und Docker Administration und Orchestrierung 

## Agenda 

  1. Grundlagen 
     * [Übersicht Architektur](architektur.md)
     * [Was ist ein Container ?](container.md)
     * [Was sind container images](container-images.md) 
     * [Container vs. Virtuelle Maschine](container-vs-vm.md)
     * [Was ist ein Dockerfile](dockerfile.md) 
  1. Installation
     * [Installation Docker unter Ubuntu mit snap](install-ubuntu-snap.md)
  1. Docker-Befehle 
     * [Die wichtigsten Befehle](docker-befehle.md)
     * [Logs anschauen - docker logs - mit Beispiel nginx](docker-logs-nginx.md)
     * [docker run](docker-run.md)
     * [Docker container/image stoppen/löschen](container-image-delete.md)
     * [Docker containerliste anzeigen](container-liste.md)
     * [Docker container analysieren](docker-inspect.md)
     * [Nginx mit portfreigabe laufen lassen](docker-example-nginx.md) 
  
  1. Dockerfile - Examples 
     * [Ubuntu mit hello world](ubuntu-hello-world.md)
     * [Ubuntu mit ping](ubuntu-ping.md) 
     * [Nginx mit content aus html-ordner](nginx-html-content.md)
     * [ssh server](ubuntu-ssh-server.md)
   
  1. Docker-Container Examples 
     * [2 Container mit Netzwerk anpingen](2-containers-with-network-ping.md)
     * [Container mit eigenem privatem Netz erstellen](container-with-own-bridge.md)  
   
  1. Daten persistent machen / Shared Volumes 
     * [Überblick](storage-overview.md) 
     * [Volumes](storage-volumes.md) 
 
  1. Netzwerk 
     * [Netzwerk](network.md)

  1. Docker Compose
     * [yaml-format](yaml-format.md)
     * [Ist docker-compose installiert?](docker-compose-installed.md) 
     * [Example with Wordpress / MySQL](example-wordpress-mysql.md)
     * [Example with Wordpress / Nginx / MariadB](example-wnm-docker-compose.md)
     * [Example with Ubuntu and Dockerfile](example-docker-compose-ubuntu-build.md)
     * [Logs in docker - compose](docker-compose-logs.md)

  1. Docker Swarm 
     * [Docker Swarm Beispiele](docker-swarm-examples.md)
 
  1. Tipps & Tricks allgemein / docker 
     * [Auf ubuntu root-benutzer werden](sudo.md)
     * [IP - Adresse abfragen](ip-a.md)
     * [Hostname setzen](hostname.md)
     * [Proxy für Docker setzen](proxy-docker.md)
     * [YAML Linter Online](http://www.yamllint.com/)
     * [Läuft der ssh-server](ssh-running.md)
     * [Basis/Parent - Image erstellen](docker-base-image.md)
     * [Eigenes unsichere Registry-Verwenden. ohne https](insecure-registry.md)

  1. Kubernetes - Überblick
     * [Warum Kubernetes, was macht Kubernetes](warum-kubernetes.md) 
     * [Aufbau](/kubernets/architecture.md)
     * [Welches System ? (minikube, micro8ks etc.)](welches-system.md)
    
  1. microk8s 
     * [Installation Ubuntu - snap](microk8s/installation-ubuntu-snap.md)
     * [Patch to next major release - cluster](microk8s/patch-next-major.md)
     * [Remote-Verbindung zu Kubernetes (microk8s) einrichten](microk8s/connect-from-remote.md)
     * [Create a cluster with microk8s](microk8s/cluster.md)
     * [Arbeiten mit der Registry](microk8s/registry.md)

  1. kubectl 
     * [Start pod (container with run && examples)](/kubectl/run-with-example.md)
     * [kubectl Spickzettle](/kubectl/spickzettel.md)

  1. kubectl - manifest - examples 
     * [02 Pod nginx mit Port und IP innerhalb des Clusters](/kubectl-examples/02-pod-nginx-exposed.md)
     * [04 Ingress mit einfachen Beispiel](/kubectl-examples/04-ingress-nginx.md)

  1. Documentation 
     * [Vulnerability Scanner with docker](https://docs.docker.com/engine/scan/#prerequisites)
     * [Vulnerability Scanner mit snyk](https://snyk.io/plans/)
     * [Parent/Base - Image bauen für Docker](https://docs.docker.com/develop/develop-images/baseimages/)
     * [Documentation zu microk8s plugins/addons](https://microk8s.io/docs/addons)
