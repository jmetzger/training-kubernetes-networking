# Node unter Wartung stellen 

```
# Achtung, bitte keine pods verwenden, dies können "ge"-drained (ausgetrocknet) werden 
kubectl drain <node-name>
z.B. 
# Daemonsets ignorieren, da diese nicht gelöscht werden 
kubectl drain n17 --ignore-daemonsets 

# Alle pods von replicasets werden jetzt auf andere nodes verschoben 
# Ich kann jetzt wartungsarbeiten durchführen 

# Wenn fertig bin:
kubectl uncordon n17 

# Achtung: deployments werden nicht neu ausgerollt, dass muss ich anstossen.
# z.B. 
kubectl rollout restart deploy/webserver 


```
