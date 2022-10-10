# Remove unused data

```
docker system prune 
# Löscht möglicherweise nicht alles

# d.h. danach nochmal prüfen ob noch images da sind
docker images 
# und händisch löschen 
docker rmi <image-name>

```
