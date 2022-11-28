# Example nginx with content

## Schritt 1: Simple Example 

```
# das gleich wie cd ~
# Heimatverzeichnis des Benutzers root 
cd
mkdir nginx-test
cd nginx-test
mkdir html
cd html/
vi index.html

```

```
Text, den du rein haben möchtest 
```

```
cd ..
vi Dockerfile 
```

```
FROM nginx:latest
COPY html /usr/share/nginx/html
```

```
# nameskürzel z.B. jm1 
docker build -t nginx-test  . 
docker images

```



## Schritt 2: docker laufen lassen

```
# und direkt aus der Registry wieder runterladen 
docker run --name hello-web -p 8080:80 -d nginx-test

# laufenden Container anzeigen lassen
docker container ls 
# oder alt: deprecated 
docker ps 

curl http://localhost:8080 


# 
docker rm -f hello-web 

```
