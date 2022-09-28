# Ubuntu HelloWorld 

```
## Schritt 1:
cd 
mkdir Hello-World 
cd Hello-World

## Schritt 2:
# nano Dockerfile
FROM ubuntu:latest 

COPY hello.sh .
RUN chmod u+x hello.sh
CMD ["/hello.sh"]

## Schritt 3:
nano hello.sh 
#!/bin/bash
echo hello-docker

## Schritt 4:
# docker build -t dockertrainereu/<dein-name>-hello-docker . 
# Beispiel
docker build -t dockertrainereu/jm-hello-docker .
docker images
docker run dockertrainereu/<dein-name>-hello-docker 

docker login
user: dockertrainereu 
pass: --bekommt ihr vom trainer--

# docker push dockertrainereu/<dein-name>-hello-docker 
# z.B. 
docker push dockertrainereu/jm-hello-docker

# und wir schauen online, ob wir das dort finden

```
