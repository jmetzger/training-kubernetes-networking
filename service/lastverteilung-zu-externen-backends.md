# Lastverteilung zu externen Backends ausserhalb von Kubernetes - Cluster

## Schaubild 

<img width="863" height="511" alt="image" src="https://github.com/user-attachments/assets/2aae6747-94ad-431e-b5e4-6a6e2f135976" />



## Manifeste 

```
---
apiVersion: v1
kind: Namespace
metadata:
  name: ext-lb
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: envoy-config
  namespace: ext-lb
data:
  envoy.yaml: |
    static_resources:
      listeners:
      - name: listener_https_passthrough
        address:
          socket_address: { address: 0.0.0.0, port_value: 443 }
        filter_chains:
        - filters:
          - name: envoy.filters.network.tcp_proxy
            typed_config:
              "@type": type.googleapis.com/envoy.extensions.filters.network.tcp_proxy.v3.TcpProxy
              stat_prefix: tcp_passthrough
              cluster: external_backends
              access_log:
              - name: envoy.access_loggers.stdout
                typed_config:
                  "@type": type.googleapis.com/envoy.extensions.access_loggers.stream.v3.StdoutAccessLog

      clusters:
      - name: external_backends
        type: STATIC
        connect_timeout: 2s
        lb_policy: ROUND_ROBIN
        # Optional: Outlier Detection nimmt instabile Hosts temporär raus
        outlier_detection:
          consecutive_5xx: 1
          interval: 5s
          base_ejection_time: 30s
          max_ejection_percent: 100
        load_assignment:
          cluster_name: external_backends
          endpoints:
          - lb_endpoints:
            - endpoint:
                address:
                  socket_address: { address: 203.0.113.10, port_value: 443 }
            - endpoint:
                address:
                  socket_address: { address: 203.0.113.11, port_value: 443 }
            - endpoint:
                address:
                  socket_address: { address: 203.0.113.12, port_value: 443 }
        health_checks:
        - timeout: 2s
          interval: 5s
          unhealthy_threshold: 2
          healthy_threshold: 2
          tcp_health_check: {}   # reine TCP-Erreichbarkeit prüfen
          # Wenn deine Backends HTTP-Health bieten, nutze stattdessen:
          # http_health_check:
          #   path: /health
          #   expected_statuses: [{start:200, end:299}]
          #   codec_client_type: HTTP1

    admin:
      address:
        socket_address: { address: 0.0.0.0, port_value: 9901 }
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: envoy
  namespace: ext-lb
spec:
  replicas: 2
  selector:
    matchLabels:
      app: envoy
  template:
    metadata:
      labels:
        app: envoy
    spec:
      containers:
      - name: envoy
        image: envoyproxy/envoy:v1.31.2
        args:
          - -c
          - /etc/envoy/envoy.yaml
          - --disable-hot-restart
        ports:
          - name: https
            containerPort: 443
          - name: admin
            containerPort: 9901
        volumeMounts:
          - name: envoy-config
            mountPath: /etc/envoy
        readinessProbe:
          httpGet: { path: /ready, port: 9901 }
          initialDelaySeconds: 3
          periodSeconds: 5
        livenessProbe:
          httpGet: { path: /server_info, port: 9901 }
          initialDelaySeconds: 10
          periodSeconds: 10
        resources:
          requests: { cpu: "50m", memory: "64Mi" }
          limits:   { cpu: "500m", memory: "256Mi" }
      volumes:
        - name: envoy-config
          configMap:
            name: envoy-config
            items:
              - key: envoy.yaml
                path: envoy.yaml
---
apiVersion: v1
kind: Service
metadata:
  name: ext-backend
  namespace: ext-lb
spec:
  type: ClusterIP
  selector:
    app: envoy
  ports:
    - name: https
      port: 443        # Port, den deine App im Pod verwendet
      targetPort: https
```


## Automatischer Neuladen bei Änderung der configMap 

  * https://github.com/stakater/Reloader

