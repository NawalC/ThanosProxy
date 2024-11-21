this does not have port defined
```sh
helm install prometheus bitnami/prometheus \
    --namespace monitoring \
    --set server.service.type=LoadBalancer \
    --set alertmanager.enabled=false \
    --set pushgateway.enabled=false
```

this does

 
    ```bash
    helm install prometheus bitnami/prometheus --namespace monitoring \
      --set server.service.type=NodePort \
      --set server.service.nodePort=9090 \
      --set alertmanager.enabled=false \
      --set pushgateway.enabled=false
    ```