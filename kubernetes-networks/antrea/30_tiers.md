# Antrea Tiers 

## Basics 

  * Antrea introduced Tiers to be able to manage firewall rules more generically
  * There are static tiers, that are already predefined (but without any NetworkPolicies)
  * They are processed by priority (lower priority-number has higher priority)
  * not namespaced (Namespaced == false)

## View Tiers 

```
kubectl get tiers 
```

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/fbcd36b2-0144-44c9-9ec5-0c65cc3ebd1b)


## Important 

  * These are readonly

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/9c67296b-3992-404e-ab2a-6b6f9a3c0273)
