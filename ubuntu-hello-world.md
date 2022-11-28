# Ubuntu HelloWorld 

## Simple Version 

### Schritt 1:
```
cd 
mkdir hello-world 
cd hello-world
```

### Schritt 2:

```
# nano Dockerfile
FROM ubuntu:latest 

COPY hello.sh .
RUN chmod u+x hello.sh
CMD ["/hello.sh"]
```

### Schritt 3:
```
nano hello.sh 
```

```
#!/bin/bash
let i=0

while true
do
  let i=i+1
  echo $i:hello-docker
  sleep 5
done
```

### Schritt 4:

```
# dockertrainereu/<dein-name>-hello-docker . 
# Beispiel
docker build -t dockertrainereu/jm-hello-docker .
docker images
docker run dockertrainereu/<dein-name>-hello-docker 
```


### Schritt 5:

```
docker login
user: dockertrainereu 
pass: --bekommt ihr vom trainer--

# docker push dockertrainereu/<dein-name>-hello-docker 
# z.B. 
docker push dockertrainereu/jm-hello-docker

# und wir schauen online, ob wir das dort finden

```

## Advanced Version 

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
while true
do 
  echo hello-docker
done 

## Schritt 4:
# docker build -t dockertrainereu/<dein-name>-hello-docker . 
# Beispiel
docker build -t dockertrainereu/jm-hello-docker .
docker images
docker run -d -t --name hello dockertrainereu/<dein-name>-hello-docker 
docker exec -it hello sh 

docker login
user: dockertrainereu 
pass: --bekommt ihr vom trainer--

# docker push dockertrainereu/<dein-name>-hello-docker 
# z.B. 
docker push dockertrainereu/jm-hello-docker

# und wir schauen online, ob wir das dort finden

```
