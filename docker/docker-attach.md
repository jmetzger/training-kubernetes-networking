# Docker - Container aus dem Hintergrund in den Vordergrund (ausgabe von stdout) bringen 

## docker attach - walkthrough 

```
docker run -d ubuntu 
1a4d...

docker attach 1a4d 

# Es ist leider mit dem Aufruf run nicht möglich, den prozess wieder in den Hintergrund zu bringen 

```

## interactiven Prozess nicht beenden (statt exit) 

```
docker run -it ubuntu bash  
# ein exit würde jetzt den Prozess beenden
# exit

# Alternativ ohne beenden (detach) 
# Geht aber nur beim start mit run -it 
CTRL + P, dann CTRL + Q 

```

## Reference: 

  * https://docs.docker.com/engine/reference/commandline/attach/
