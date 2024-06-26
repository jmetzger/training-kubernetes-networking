# Kubernetes Networking 

## Agenda 

  1. Kubernetes - Überblick
     * [Aufbau Allgemein](/kubernetes/architecture.md)
     * [Structure Kubernetes Deep Dive](https://github.com/jmetzger/training-kubernetes-advanced/assets/1933318/1ca0d174-f354-43b2-81cc-67af8498b56c)
     * [CRI - Container Runtime interface](/kubernetes/internals/cri.md)
     * [Ports und Protokolle](https://kubernetes.io/docs/reference/networking/ports-and-protocols/)
     
  1. Kubernetes - Misc 
     * [Wann wird podIP vergeben ?](kubectl/run-with-example.md)
     * [Bash completion installieren](kubectl/bash-completion.md)
     * [Remote-Verbindung zu Kubernetes (microk8s) einrichten](microk8s/connect-from-remote.md)
     * [kubectl verbindung mit namespace einrichten](kubectl/kubectl-einrichten.md)
     * [vim support for yaml](vim/vim-yaml.md)
     
  1. Kubernetes - Netzwerk (CNI's) / Mesh
     * [Netzwerk Interna](/kubernetes-networks/networking-internal-overview.md)
     * [Wirkweise cni](/kubernetes/internals/cni.md)
     * [Übersicht Netzwerke](/kubernetes-networks/overview.md) 
     * [Calico/Cilium - nginx example NetworkPolicy](kubernetes-networkpolicy/00-simple-exercises-group.md)
     * [Beispiele Ingress Egress NetworkPolicy](kubernetes-networks/examples-ingress-egress.md)
     * [Mesh / istio](sammlung-istio.md)  
     * [Kubernetes Ports/Protokolle](https://kubernetes.io/docs/reference/networking/ports-and-protocols/)
     * [IPV4/IPV6 Dualstack](https://kubernetes.io/docs/concepts/services-networking/dual-stack/)
     * [DNS - Resolution - Services](kubernetes-networks/dns-resolution-services.md)
     * [Debug Container](kubernetes-networks/debug-container.md)

  1. Kubernetes NetworkPolicy
     * [Einfache Übung Network Policy](kubernetes-networkpolicy/00-simple-exercises-group.md)

  1. Calico NetworkPolicy
     * [Protecting Services](kubernetes-networkpolicy/calico/02-example-protecting-services.md)
     * [Exercise calico Network Policy](kubernetes-networkpolicy/calico/01-exercise-calico.md)
 
  1. Kubernetes antrea (CNI-Plugin)
     * [Unterschiede Dokus vmware (antrea mit nsx-t) und OpenSource Antrea](kubernetes-networks/antrea/00_documentation-open-source-vs-vmware.md)
     * [Overview Kubernetes Antrea CNI-Plugin](kubernetes-networks/antrea/01_overview.md)
     * [Antctl](kubernetes-networks/antrea/10_antctl.md)
     * [Antrea view bridge and config](kubernetes-networks/antrea/20-view-bridge-and-config.md)
     * [Antrea Exercise](kubernetes-networks/antrea/99_exercise.md)
  
  1. Kubernetes calico (CNI-Plugin)
     * [Welcher Routing-Mode wird im aktuellen Cluster verwendet](/kubernetes-networks/calico/which-routing-mode-is-used.md)
     * [Wird eBPF verwendet ?](/kubernetes-networks/calico/is-ebpf-used.md)
     * [Install calicoctl in pod](/kubernetes-networks/calicoctl-installation.md)
     * [Wann calicoctl (Stand 2024/01 calico 3.27)](calico/wann-calicoctl.md)
     * [Install calico-api-server to use kubectl instead of calicoctl](kubernetes-networks/calico/install-calico-api-server.md)
     * [Find corresponding networks](kubernetes-networks/calico/find-corresponding-interfaces.md)
     * [Calico Logging Firewall Rules](kubernetes-networks/calico/logging-policy.md)
     * [Calico Default Routing Mode BGP & vxlancrossnet](kubernetes-networks/calico/routing-modes/bgp-vxlancrossnet.md)
     * [Internals - Pod to Pod - Communication on Worker3 (node))](kubernetes-networks/calico/pod-2-pod-on-node-internals.md)
     * [Internals - Inter-Pod - Communication (worker 3 -> worker 1](kubernetes-networks/calico/routing-modes/vxlan/internals-routing-inter-pod-communication.md)

  1. Kubernetes multus (Meta-CNI - Plugin)
     * [Multus Überblick](kubernetes-networks/multus/overview.md)
     * [sr-iov mit multus](kubernetes-networks/multus/sr-iov.md)

  1. Kubernetes coil (egress - gateway)
     * [coil](kubernetes-networks/coil/overview.md)

  1. Kubernetes - Ingress
     * [Vom Browser über den Ingress bis zum Pod - Schaubild](/kubernetes/ingress/schaubild.md)
     * [Ingress controller in microk8s aktivieren](microk8s/ingress.md) 
     * [ingress mit ssl absichern](/kubernetes-security/ingress-ssl.md)

  1. Kubernetes - Wartung / Debugging 
     * [kubectl drain/uncordon](/kubectl/uncordon-drain.md)
     * [Alte manifeste konvertieren mit convert plugin](/kubectl/convert-plugin.md)
     * [Netzwerkverbindung zu pod testen](/tipps-tricks/verbindung-zu-pod-testen.md)
     * [Curl from pod api-server](/kubernetes-advanced/curl-api-server.md)

  1. Kubernetes Praxis API-Objekte 
     * [Das Tool kubectl (Devs/Ops) - Spickzettel](/kubectl/spickzettel.md)
     * [kubectl example with run](/kubectl/run-with-example.md)
     * [Bauen einer Applikation mit Resource Objekten](bauen-einer-webanwendung.md)
     * [Pod manifest](kubectl-examples/01-pod-nginx.md)
     * [Replicasets](kubectl-examples/01a-replicaset-nginx.md)
     * [kubectl/manifest/deployments](/kubectl-examples/03-nginx-deployment.md)
     * [Services - Aufbau](/kubernetes/services-aufbau.md)
     * [Service Typen / Ebenen - Schaubild](service-ebenen.md)
     * [kubectl/manifest/service](/kubectl-examples/03b-service.md)
     * DaemonSets (Devs/Ops)
     * [Hintergrund Ingress](/kubernetes/ingress.md) 
     * [Ingress Controller auf Digitalocean (doks) mit helm installieren](/digitalocean/ingress-auf-digitalocean-mit-helm.md)
     * [Documentation for default ingress nginx](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/)
     * [Beispiel Ingress](/kubectl-examples/04-ingress-nginx.md)
     * [Install Ingress On Digitalocean DOKS](/digitalocean/install-ingress-helm.md)
     * [Beispiel mit Hostnamen](/kubectl-examples/04-ingress-nginx-with-hostnames.md)
     * [Achtung: Ingress mit Helm - annotations](/ingress-mit-helm-class-achtung.md)
     * [Permanente Weiterleitung mit Ingress](/kubectl-examples/05-ingress-permanent-redirect.md)
     * [ConfigMap Example](/kubectl-examples/06-configmap.md)
     * [Configmap MariaDB - Example](kubectl-examples/06a-configmap-mariadb.md)
     * [Configmap MariaDB my.cnf](kubectl-examples/06b-mariadb-configmap-configfile.md)     
  
  1. Kubernetes Deployment Scenarios 
     * [Deployment green/blue,canary,rolling update](/kubernetes/deployment-strategies-en.md)
     * [Service Blue/Green](/kubectl-examples/03c-service-blue-green-nginx.md)
     * [Praxis-Übung A/B Deployment](/kubectl-examples/08-ab-deployment.md)   
     
  1. Helm (Kubernetes Paketmanager) 
     * [Helm Grundlagen](/helm/grundlagen.md)
     * [Helm Warum ?](/helm/warum.md)
     * [Helm Example](/helm/example.md)

  1. Kubernetes - RBAC 
     * [Nutzer einrichten microk8s ab kubernetes 1.25](/kubernetes/rbac-create-user-kubernetes-1-25.md)
     * [Tipps&Tricks zu Deploymnent - Rollout](/kubectl/rollout.md) 

  1. Kubernetes QoS 
     * [Quality of Service - evict pods](kubernetes/qos-class.md)

  1. Kustomize 
     * [Kustomize Overlay Beispiel](/kustomize/02-overlay-example.md)
     * [Helm mit kustomize verheiraten](/kustomize/helm-kustomize-options.md)

  1. Kubernetes - Tipps & Tricks 
     * [Kubernetes Debuggen ClusterIP/PodIP](/tipps-tricks/cluster-ip-debug.md)
     * [Debugging pods](tipps-tricks/debugging-pods.md)
     * [Taints und Tolerations](kubernetes/taints-tolerations.md)
     * [Autoscaling Pods/Deployments](/kubernetes/autoscaling.md)
     * [pod aus deployment bei config - Änderung neu ausrollen](https://github.com/stakater/Reloader)

  1. Kubernetes Advanced 
     * [Curl api-server kubernetes aus pod heraus](kubernetes-advanced/curl-api-server.md)

  1. Kubernetes - Documentation 
     * [Documentation zu microk8s plugins/addons](https://microk8s.io/docs/addons)  
     * [Shared Volumes - Welche gibt es ?](https://kubernetes.io/docs/concepts/storage/volumes/)

  1. Kubernetes - Hardening 
     * [Kubernetes Tipps Hardening](kubernetes-security/tipps-hardening.md)
     * [Kubernetes Security Admission Controller Example](kubernetes-security/pod-security-admission.md)
     * [Was muss ich bei der Netzwerk-Sicherheit beachten ?](kubernetes-security/network-tasks-security-overview.md)
     
  1. Kubernetes Interna / Misc.
     * [OCI,Container,Images Standards](docker-alternatives-kubernetes.md)
     * [Geolocation Kubernetes Cluster](https://learnk8s.io/bite-sized/connecting-multiple-kubernetes-clusters)
     * [statische IP für Pod in calico](https://docs.tigera.io/calico/latest/networking/ipam/use-specific-ip)
     * [yaml linting](https://www.kubeval.com/installation/)
     * [ssl terminierung über proxy nginx](ssl-nginx-termination.md)
     * [LoadBalancer / Cluster Controller Manager](/internals/service/loadbalancer/cloud-controller-manager-do.md)
   
  1. Kubernetes Load Balancer 
     * [Kubernetes Load Balancer](metallb.md)
     * [Kubernetes Load Balancer new version for IpAdresses - object](loadbalancer/metallb.md)
 
  1. Kubernetes Documentation 
     * [Well-Known Annotations](https://kubernetes.io/docs/reference/labels-annotations-taints/)
  
## Backlog

  1. Kubernetes - Überblick
     * [Installation - Welche Komponenten from scratch](/kubernetes/installation-components-overview.md)

  1. Kubernetes - microk8s (Installation und Management) 
     * [kubectl unter windows - Remote-Verbindung zu Kuberenets (microk8s) einrichten](kubectl-windows.md)
     * [Arbeiten mit der Registry](microk8s/registry.md)
     * [Installation Kubernetes Dashboard](/microk8s/dashboard.md) 

  1. Kubernetes - RBAC 
     * [Nutzer einrichten - kubernetes bis 1.24](/kubernetes/rbac-create-user.md) 
     
  1. kubectl 
     * [Tipps&Tricks zu Deploymnent - Rollout](/kubectl/rollout.md) 
     
  1. Kubernetes - Monitoring (microk8s und vanilla) 
     * [metrics-server aktivieren (microk8s und vanilla)](/microk8s/metrics-server.md)

  1. Kubernetes - Backups 
     + [Kubernetes Aware Cloud Backup - kasten.io](/backups/cluster-backup-kasten-io.md)

  1. Kubernetes - Tipps & Tricks 
     * [Assigning Pods to Nodes](/tipps-tricks/pods-2-nodes.md) 

  1. Kubernetes - Documentation 
     * [LDAP-Anbindung](https://github.com/apprenda-kismatic/kubernetes-ldap)
     * [Helpful to learn - Kubernetes](https://kubernetes.io/docs/tasks/)
     * [Environment to learn](https://killercoda.com/killer-shell-cks)
     * [Environment to learn II](https://killercoda.com/)
     * [Youtube Channel](https://www.youtube.com/watch?v=01qcYSck1c4)

  1. Kubernetes - Shared Volumes 
     * [Shared Volumes with nfs](shared-volumes/nfs-multi.md)

  1. Kubernetes - Hardening 
     * [Kubernetes Tipps Hardening](kubernetes-security/tipps-hardening.md)

  1. Kubernetes Probes (Liveness and Readiness) 
     * [Übung Liveness-Probe](/probes/uebung-liveness.md)
     * [Funktionsweise Readiness-Probe vs. Liveness-Probe](/probes/readiness.md) 
       

     

