# Network 

## Übersicht

```
3 Typen 

o none
o bridge (Standard-Netzwerk) 
o host 

## Additionally possible to install
o overlay (needed for multi-node)

```


## Kommandos 

```
# Netzwerk anzeigen 
docker network ls 

# bridge netzwerk anschauen 
# Zeigt auch ip der docker container an  
docker inspect bridge

# im container sehen wir es auch
docker inspect ubuntu-container 

```

## Eigenes Netz erstellen 

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
