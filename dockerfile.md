# Dockerfile 

# What is it ? 
 * Textdatei, die Linux - Kommandos enthält
   * die man auch auf der Kommandozeile ausführen könnte 
   * Diese erledigen alle Aufgaben, die nötig sind, um ein Image zusammenzustellen
   * mit docker build wird dieses image erstellt 
   
# Exmaple 

```
# syntax=docker/dockerfile:1
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```
