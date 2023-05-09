# Why overlay networks 

## General 

  * We have one subnet per Node 
  * This is taken from the complete network of our cluster 

## Why overlay network ? 

  * We need to be able to communicate between pods und different nodes 
  * One possibility is to:
    * use encapsulation (vxlan) and encapsulate the packet into another packet.
    * These packets are udp packets 
    * By doing so i can reach pods on other hosts 

## Alternatives
 
  * This is one possiblity, but how do others work ? 
