# Curl mit Hilfe eines Containers verwenden

## Situation 

  * Kein Zugriff auf die Nodes, zum Testen von Verbindungen zu Pods und Services über die ClusterIP 

## Lösung 

```
# Achtung http:// muss angegeben werden, sonst funktioniert das Kommando möglichweiser nicht
# -L sollte man immer verwenden, leitet um 
# --output - gibt es auf stdout (Bildschirm aus) 
kubectl run -it --rm --image=curlimages/curl curly -- curl -L --output - http://www.test.de 

```
