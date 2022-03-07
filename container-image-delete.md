# Container - Image - Delete - Kill

```
docker stop ubuntu-container 
# Kill it if it cannot be stopped -be careful
docker kill ubuntu-container

# Get nur, wenn der Container nicht mehr läuft 
docker rm ubuntu-container

# oder alternative
docker rm -f ubuntu-container 


# image löschen 
docker rmi ubuntu:xenial 

# falls Container noch vorhanden aber nicht laufend 
docker rmi -f ubuntu:xenial 

```
