# Example Docker Compose (Ubuntu with Dockerfile) 

```
cd
mkdir bautest
cd bautest 
```

```
# nano docker-compose.yml
version: "3.8"

services:
  myubuntu:
    build: ./myubuntu
    restart: always
```

```
mkdir myubuntu 
cd myubuntu 
```

```
# nano Dockerfile 
FROM ubuntu:latest
RUN apt-get update; apt-get install -y inetutils-ping
CMD ["/bin/bash"]
```

```
cd ../
# wichtig, im docker-compose - Ordner seiend 
#pwd 
#~/bautest
docker-compose up -d 
# wird image gebaut und container gestartet 

# Bei Veränderung vom Dockerfile, muss man den Parameter --build mitangeben 
docker-compose up -d --build 
```
