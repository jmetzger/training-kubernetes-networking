# Taints and Tolerations 

## Taints 

```
Taints schliessen auf einer Node alle Pods aus, die nicht bestimmte label haben:

Möglichkeiten:

o Sie werden nicht gescheduled - noschedule 
o Sie werden nicht executed - no execute 
o Sie werden möglichst nicht gescheduled. 


```

## Tolerations 

```
Tolerations werden auf Pod-Ebene vergeben: 
tolerations: 

Ein Pod kann (wenn es auf einem Node taints gibt), nur 
gescheduled bzw. ausgeführt werden, wenn er die 
Labels hat, die auch als
Taints auf dem Node vergeben sind.
```

## Taints vergeben 

```



```


## Taints rausnehmen 

## References 
  
  * [Doku Kubernetes Taints and Tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)
  * https://blog.kubecost.com/blog/kubernetes-taints/
