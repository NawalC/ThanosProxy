
# Task Markdown

## 1. Markdown Instructions (`/workspaces/ThanosProxy/5_Formula/config.md`)

To set up Helm, Prometheus, and other components, follow the steps below:

### Steps

1. **Start Minikube**
    ```sh
    minikube start
    ```

2. **Check Minikube Status**
    ```sh
    minikube status
    ```

3. **Add Bitnami Helm Repository**
    ```sh
    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm repo update
    ```

4. **Create Namespace**
    ```sh
    kubectl create namespace monitoring
    ```

5. **Install Prometheus**
Depending on your use case, you can install Prometheus with either a NodePort or a LoadBalancer service type.

**Using LoadBalancer:**
```sh
helm install prometheus bitnami/prometheus --namespace monitoring \
  --set server.service.type=LoadBalancer \
  --set alertmanager.enabled=true \
  --set pushgateway.enabled=true
```

**Using NodePort:**
```sh
helm install prometheus bitnami/prometheus --namespace monitoring \
  --set server.service.type=NodePort \
  --set server.service.nodePort=30000 \
  --set alertmanager.enabled=true \
  --set pushgateway.enabled=true
```

6. **Access Prometheus**

    Depending on the service type you chose, access Prometheus using the appropriate method:

    **For LoadBalancer:**
    ```sh
    kubectl get svc --namespace monitoring -w prometheus-server
    ```

    **For NodePort:**
    ```sh
    minikube service prometheus-server --namespace monitoring
    ```

7. **Port-Forward to Prometheus**
    ```sh
    kubectl port-forward svc/prometheus-server 9090:9090 --namespace monitoring
    ```

8. **Configure Prometheus**

    Update the Prometheus configuration file `/workspaces/ThanosProxy/5_Formula/prometheus-config.yaml` with the provided content.

8. **Install Thanos**
    ```sh
    helm install thanos bitnami/thanos --namespace monitoring \
      --set query.enabled=true \
      --set storegateway.enabled=true \
      --set bucketConfig.secretName=thanos-bucket-config \
      --set bucketConfig.secretKey=bucket.yaml
    ```

9. **Create Bucket Configuration**
    ```sh
    kubectl create secret generic thanos-bucket-config --from-file=bucket.yaml --namespace monitoring
    ```

10. **Install Query Frontend**
    ```sh
    helm install thanos-query bitnami/thanos --namespace monitoring \
      --set queryFrontend.enabled=true \
      --set queryFrontend.replicaCount=2 \
      --set queryFrontend.downstream.url=http://thanos-query:9090
    ```

11. **Install Storage Gateway**
    ```sh
    helm install thanos-storegateway bitnami/thanos --namespace monitoring \
      --set storegateway.enabled=true \
      --set bucketConfig.secretName=thanos-bucket-config
    ```

12. **Install Thanos Ruler**
    ```sh
    helm install thanos-ruler bitnami/thanos --namespace monitoring \
      --set ruler.enabled=true \
      --set ruler.alertmanagers.url=http://prometheus-alertmanager.default.svc.cluster.local:9093
    ```

13. **Verify Installation**
    ```sh
    helm list --namespace monitoring
    kubectl get pods --namespace monitoring
    ```

14. **Access Services**
    ```sh
    kubectl port-forward svc/thanos-query 9091:9090 --namespace monitoring
    ```

15. **Configure Proxy Settings**

    Create a ConfigMap for the proxy configuration if needed:
    ```sh
    kubectl apply -f /workspaces/ThanosProxy/5_Formula/proxy-config.yaml
    ```

16. **Expose Services**

    Optionally, configure Ingress or use LoadBalancer services to expose Prometheus and Thanos services.

**Summary:**  
By following these steps, you will have a complete setup of Helm, Prometheus, and Thanos with the necessary configurations.

---

## 2. Prometheus Configuration (`/workspaces/ThanosProxy/5_Formula/prometheus-config.yaml`)
```yaml
# Prometheus configuration file
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'pushgateway'
    static_configs:
      - targets: ['pushgateway:9091']

alerting:
  alertmanagers:
    - static_configs:
        - targets: ['alertmanager:9093']
```

---

## 3. Proxy Configuration (`/workspaces/ThanosProxy/5_Formula/proxy-config.yaml`)
```yaml
# Proxy configuration file
apiVersion: v1
kind: ConfigMap
metadata:
  name: proxy-config
  namespace: monitoring
data:
  proxy.conf: |
    # Proxy configuration settings
```

