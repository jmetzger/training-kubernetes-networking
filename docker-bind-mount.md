# Example 

```
# andere Verzeichnis als das Heimatverzeichnis von root funktionieren aktuell nicht mit 
# snap install docker 
# wg. des Confinements 
docker run -d -it  --name devtest --mount type=bind,source=/root,target=/app nginx:latest
docker exec -it devtest bash 
/# cd /app 
```
