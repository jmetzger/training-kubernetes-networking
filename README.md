# Kubernetes und Docker Administration und Orchestrierung 

## Agenda 

  1. Docker-Grundlagen 
     * [Übersicht Architektur](architektur.md)
     * [Was ist ein Container ?](container.md)
     * [Was sind container images](container-images.md) 
     * [Container vs. Virtuelle Maschine](container-vs-vm.md)
     * [Was ist ein Dockerfile](dockerfile.md) 
  
  1. Docker-Installation
     * [Installation Docker unter Ubuntu mit snap](install-ubuntu-snap.md)
  
  1. Docker-Befehle 
     * [Die wichtigsten Befehle](docker-befehle.md)
     * [Logs anschauen - docker logs - mit Beispiel nginx](docker-logs-nginx.md)
     * [docker run](docker-run.md)
     * [Docker container/image stoppen/löschen](container-image-delete.md)
     * [Docker containerliste anzeigen](container-liste.md)
     * [Docker container analysieren](docker-inspect.md)
     * [Docker container in den Vordergrund bringen - attach](/docker/docker-attach.md) 
     * [Aufräumen - container und images löschen](prune-container-images.md)
     * [Nginx mit portfreigabe laufen lassen](docker-example-nginx.md)
    
  
  1. Dockerfile - Examples 
     * [Ubuntu mit hello world](ubuntu-hello-world.md)
     * [Ubuntu mit ping](ubuntu-ping.md) 
     * [Nginx mit content aus html-ordner](nginx-html-content.md)
     * [ssh server](ubuntu-ssh-server.md)
  
  1. Docker-Container Examples 
     * [2 Container mit Netzwerk anpingen](2-containers-with-network-ping.md)
     * [Container mit eigenem privatem Netz erstellen](container-with-own-bridge.md)  
  
  1. Docker-Daten persistent machen / Shared Volumes 
     * [Überblick](storage-overview.md) 
     * [Volumes](storage-volumes.md) 
  
  1. Docker-Netzwerk 
     * [Netzwerk](network.md)
  
  1. Docker Compose
     * [yaml-format](yaml-format.md)
     * [Ist docker-compose installiert?](docker-compose-installed.md) 
     * [Example with Wordpress / MySQL](example-wordpress-mysql.md)
     * [Example with Wordpress / Nginx / MariadB](example-wnm-docker-compose.md)
     * [Example with Ubuntu and Dockerfile](example-docker-compose-ubuntu-build.md)
     * [Logs in docker - compose](docker-compose-logs.md)
     * [docker-compose und replicas](docker-compose-replicas.md)
  
  1. Docker Swarm 
     * [Docker Swarm Beispiele](docker-swarm-examples.md)

  1. Docker - Dokumentation 
     * [Vulnerability Scanner with docker](https://docs.docker.com/engine/scan/#prerequisites)
     * [Vulnerability Scanner mit snyk](https://snyk.io/plans/)
     * [Parent/Base - Image bauen für Docker](https://docs.docker.com/develop/develop-images/baseimages/)
    
  1. Kubernetes - Überblick
     * [Warum Kubernetes, was macht Kubernetes](warum-kubernetes.md) 
     * [Aufbau](/kubernetes/architecture.md)
     * [Welches System ? (minikube, micro8ks etc.)](welches-system.md)
     * [Installation - Welche Komponenten from scratch](/kubernetes/installation-components-overview.md)

  1. Kubernetes - microk8s (Installation und Management) 
     * [Installation Ubuntu - snap](microk8s/installation-ubuntu-snap.md)
     * [Patch to next major release - cluster](microk8s/patch-next-major.md)
     * [Remote-Verbindung zu Kubernetes (microk8s) einrichten](microk8s/connect-from-remote.md)
     * [Create a cluster with microk8s](microk8s/cluster.md)
     * [Ingress controller in microk8s aktivieren](microk8s/ingress.md) 
     * [Arbeiten mit der Registry](microk8s/registry.md)
     * [Installation Kuberenetes Dashboard](/microk8s/dashboard.md) 

  1. Kubernetes - API - Objekte
     * [Welche API-Objekte gibt es? (Kommando)](/kubernetes/api-resources.md)
     * [Api Versionierung Lifetime](/kubernetes/api-versionierung-lifetime.md)
     * [Was sind Deployments](/kubernetes/deployments.md)
     * [Service - Objekt und IP](/kubernetes/service.md)
      
  1. kubectl 
     * [Start pod (container with run && examples)](/kubectl/run-with-example.md)
     * [Bash completion for kubectl](/kubectl/bash-completion.md)
     * [kubectl Spickzettel](/kubectl/spickzettel.md)
     * [Tipps&Tricks zu Deploymnent - Rollout](/kubectl/rollout.md) 

  1. kubectl - manifest - examples 
     * [02 Pod nginx mit Port und IP innerhalb des Clusters](/kubectl-examples/02-pod-nginx-exposed.md)
     * [03b Example with service and nginx](/kubectl-examples/03b-service.md)
     * [04 Ingress mit einfachem Beispiel](/kubectl-examples/04-ingress-nginx.md)

  1. Kubernetes - Monitoring (microk8s) 
     * [metrics-server aktivieren (microk8s)](/microk8s/metrics-server.md)

  1. Kubernetes - Backups 
     + [Kubernetes Aware Cloud Backup - kasten.io](/backups/cluster-backup-kasten-io.md)

  1. Kubernetes - Wartung 
     * [kubectl drain/uncordon](/kubectl/uncordon-drain.md)

  1. Kubernetes - Documentation 
     * [Documentation zu microk8s plugins/addons](https://microk8s.io/docs/addons)

  1. Linux und Docker Tipps & Tricks allgemein 
     * [Auf ubuntu root-benutzer werden](sudo.md)
     * [IP - Adresse abfragen](ip-a.md)
     * [Hostname setzen](hostname.md)
     * [Proxy für Docker setzen](proxy-docker.md)
     * [vim einrückung für yaml-dateien](/vim/vim-yaml.md)
     * [YAML Linter Online](http://www.yamllint.com/)
     * [Läuft der ssh-server](ssh-running.md)
     * [Basis/Parent - Image erstellen](docker-base-image.md)
     * [Eigenes unsichere Registry-Verwenden. ohne https](insecure-registry.md)
