# Curl mit Hilfe eines Containers verwenden

## Situation 

  * Kein Zugriff auf die Nodes, zum Testen von Verbindungen zu Pods und Services über die PodIP/ClusterIP 

## Lösung 

```
# Wir starten eine Busybox und fragen per wget und port ab
# busytester ist der name 
# long version 
kubectl run -it --rm --image=busybox busytester 
# wget <pod-ip-des-ziels> 
# exit 


# quick and dirty 
kubectl run -it --rm --image=busybox busytester -- wget <pod-ip-des-ziels>  

```
