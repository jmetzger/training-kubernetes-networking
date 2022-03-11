# Monitoring with Elasticsearch (Fluentd) - EFK - Stack -> microk8s 

## What is fluentd ? 

  * fluentd aggregates different data like (app logs, systems logs a.s.o) - see References 

## Enable it:

```
# With microk8s you can enable this stack. 
microk8s enable fluentd 




```

## Problems that need to be fixed with microk8s 

```
# as of 04/2022 there seems some misleading information in the docs
# that keeps it from working (when it comes to access it) 
Refer to:
https://discuss.kubernetes.io/t/add-on-fluentd/11262

How to fix ? (Excerpt) 



```

## References:

  *
  * https://www.fluentd.org/architecture

## Alternatives (set it up step by step) 

  * https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes
  * 
