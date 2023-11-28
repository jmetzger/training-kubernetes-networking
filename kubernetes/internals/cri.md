# Container Runtime Interface (CRI) 

## Where is it embedded 

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/7f54d63d-6337-41e9-9ab6-8323f12b290e)

## What is it for ?

  * Abstraction layer called by kubelet to make it possible to use other container runtimes 
  * The CRI uses gRPC as its communication protocol.

## kubelet calls the CRI with its subcommands 

  * Expected commands are

```
Sandbox:
  Delete
  Create
  List
Image:
  Pull
  List
Container.
  Create
  Start
  Exec
```

## Steps in the CRI 

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/4f38ac6a-a221-4257-8a36-cfcdbb18b254)
