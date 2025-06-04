# Overview 

## Features 

  * Responsibility separation
  * TrafficRouting based on RequestHeader / Environment Variable 
  * LoadBalancing (Gewichtung: 10% an Service 1, 90% an Service 2) 
  * TCP und gRPC-Routing 

## Komponenten 

  1. GatewayController (Kong, Nginx, Traefik, HAProxy, Istio) - Software
  1. GatewayClass (Stable)
  1. Gateway (Stable) 
  1. HttpRoute (GA) / TCPRoute (experimentell) / gRPCRoute (experimentell) 

## Shared responsibility 

### Gateway API can be split into different responsibility roles

![Hallo](images/resource-model.png)

### Reference 

  * https://gateway-api.sigs.k8s.io/
