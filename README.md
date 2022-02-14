# Kubernetes und Docker Administration und Orchestrierung 

## Agenda 

  1. Grundlagen 
     * [Übersicht Architektur](architektur.md)
     * [Was ist ein Container ?](container.md)
     * [Container vs. Virtuelle Maschine](container-vs-vm.md)
  1. Installation
     * [Installation Docker unter Ubuntu mit snap](install-ubuntu-snap.md)
  1. Docker-Befehle 
     * [Die wichtigsten Befehle](docker-befehle.md)
     * [docker run](docker-run.md)
     * [Docker container/image stoppen/löschen](container-image-delete.md)
     * [Docker containerliste anzeigen](container-liste.md)
     * [Docker container analysieren](docker-inspect.md)
     * [Nginx mit portfreigabe laufen lassen](docker-example-nginx.md) 
  
  1. Dockerfile - Examples 
     * [Ubuntu mit ping](ubuntu-ping.md) 
     * [Nginx mit content aus html-ordner](nginx-html-content.md)
     * [ssh server](ubuntu-ssh-server.md)
   
  1. Daten persistent machen / Shared Volumes 
     * [Überblick](storage-overview.md) 
     * [Volumes](storage-volumes.md) 
 
  1. Netzwerk 
     * [Netzwerk](network.md)

  1. Docker Compose 
     * [Example with Wordpress / Nginx / MariadB](example-wnm-docker-compose.md)
     * [Example with Ubuntu and Dockerfile](example-docker-compose-ubuntu-build.md)
     * [Logs in docker - compose](docker-compose-logs.md)

  1. Docker Swarm 
     * [Docker Swarm Beispiele](docker-swarm-examples.md)
 
  1. Tipps & Tricks 
     * [Auf ubuntu root-benutzer werden](sudo.md)
     * [IP - Adresse abfragen](ip-a.md)
     * [Hostname setzen](hostname.md)
     * [Proxy für Docker setzen](proxy-docker.md)
     * [YAML Linter Online](http://www.yamllint.com/)

  1. Documentation 
     * [Vulnerability Scanner with docker](https://docs.docker.com/engine/scan/#prerequisites)
     * [Vulnerability Scanner mit snyk](https://snyk.io/plans/)
