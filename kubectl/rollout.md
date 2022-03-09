# Kubectl Rollout 

## Warum 

```
Rückgängig machen von deploys, Deploys neu unstossen.
(Das sind die wichtigsten Fähigkeiten

```

## Beispiele 

```
# Deployment nochmal durchführen 
# z.B. nach kubectl uncordon n12.training.local 
kubectl rollout restart deploy nginx-deployment 

# Rollout rückgängig machen 
kubectl rollout undo deploy nginx-deployment 

```
