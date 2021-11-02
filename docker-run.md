# Docker run 

## Beispiel (binden an ein terminal), detached

```
# before that we did
docker pull ubuntu:xenial
docker run -t -d --name my_xenial ubuntu:xenial
# will wollen überprüfen, ob der container läuft
docker container ls 
# image vorhanden 
docker images

```
