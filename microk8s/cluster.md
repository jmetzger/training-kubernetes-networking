# Create a cluster 

## Walkthrough 

```
# auf master (jeweils für jedes node neu ausführen)
microk8s add-node

# dann auf jeweiligem node vorigen Befehl der ausgegeben wurde ausführen
# Kann mehr als 60 sekunden dauern ! Geduld...Geduld..Geduld 
#z.B. -> ACHTUNG evtl. IP ändern 
microk8s join 10.128.63.86:25000/567a21bdfc9a64738ef4b3286b2b8a69

```

## Auf einem Node addon aktivieren z.B. ingress

```
gucken, ob es auf dem anderen node auch aktiv ist. 
```

## Ref:

  * https://microk8s.io/docs/high-availability
