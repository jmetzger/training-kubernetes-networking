# Multus mit sr-iov 

## Voraussetzung: Multus:

## Konzept SR-IOV

  * Direkte Hardwareanbindung der Netzwerkkarte
  * Offload wird auf Netzwerkkarte gemacht (nicht im Kernel)
  * bessere Performance

## Generell 

  * Erweiterung des PCI-Express Standards 
  * Eine Netzwerkkarte wird mehrmals angeboten und Kommunikation erfolgt direkt und nicht über den Umweg Kernel

## Vorbereitung

 * https://github.com/k8snetworkplumbingwg/sriov-network-device-plugin
 * https://github.com/k8snetworkplumbingwg/sriov-network-device-plugin/tree/db98d96cc0d6ad3fff917ba238bd1cc5cc3f7e82#config-parameters

## Einbindung 

  * https://github.com/k8snetworkplumbingwg/sriov-network-device-plugin#example-deployments
  * https://github.com/k8snetworkplumbingwg/multus-cni/blob/master/examples/sriov-pod.yml



 

