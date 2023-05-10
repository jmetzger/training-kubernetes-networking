# Absichern Netzwerk (und darüber hinaus) 

## Bereich 1: Kubernetes (Cluster) 


```
1. Welche Ports sollten wirklich geöffnet sein ?

für Kubernetes 

2. Wer muss den von wo den Kube-Api-Server zugreifen

- den Traffic einschränken 

```

## Bereich 2: Nodes 

```
Alle nicht benötigten fremden Ports sollten geschlossen sein 
Wenn offen, nur über vordefinierte Zugangswege (und auch nur bestimmte Nutzer) 
```

## Pods (Container / Image)

```
# Ingress (NetworkPolicy) - engmaschig stricken 
# 1. Wer soll von wo auf welche Pod zugreifen können 

# 2. Welche Pod auf welchen anderen Pod (Service) 

ä Egress 
# Welche Pods dürfen wohin nach draussen 

```

## Einschränking der Fähigkeien eines Pods 

```
kein PrivilegeEscalation 
nur notwendige Capabilities 
unter einem nicht-root Benutzer laufen lassen
...

## Patching 

```
# pods -> neuestes images bei security vulnerablities 
# nodes -> auch neues patches (apt upgrade) 
# kubernetes cluster -> auf dem neuesten Stand 
  # -> wie ist der Prozess ClusterUpdate, update der manifeste zu neuen API-Versionen 

```

## RBAC 

```
# Nutzer (kubectl, systemnutzer -> pods) 

# 1. Zugriff von den pods 


# 2. Zugriff über helm / kubectl  
# Wer darf was ? Was muss der Nutzer können 
```

## Compliance 

```
PSP's / PSA 
PodSecurityPolicy was deprecated in Kubernetes v1.21, and removed from Kubernetes in v1.25

PSA - Pode Security Admission
```

