# Deploying Prometheus and Thanos Monitoring Stack
## Environments
## Deployment Steps

Follow these simple commands to deploy Prometheus and Thanos using Bitnami Helm charts:

1. **Add Bitnami Helm Repository**
    ```bash
    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm repo update
    ```
    **Check if helm is installed**
    ```bash
    helm version
    helm list
    ```

2. **Install Prometheus**
    ```bash
    helm install prometheus bitnami/prometheus --namespace monitoring --set server.service.type=LoadBalancer --set alertmanager.enabled=false --set pushgateway.enabled=false
    ```

3. **Install Thanos**
    ```bash
    helm install thanos bitnami/thanos \
      --set query.enabled=true \
      --set storegateway.enabled=true \
      --set bucketConfig.secretName=thanos-bucket-config \
      --set bucketConfig.secretKey=bucket.yaml
    ```

4. **Create Bucket Configuration**
    ```bash
    kubectl create secret generic thanos-bucket-config \
      --from-file=bucket.yaml
    ```

5. **Install Query Frontend**
    ```bash
    helm install thanos-query bitnami/thanos \
      --set queryFrontend.enabled=true \
      --set queryFrontend.replicaCount=2 \
      --set queryFrontend.downstream.url=http://thanos-query:9090
    ```

6. **Install Storage Gateway**
    ```bash
    helm install thanos-storegateway bitnami/thanos \
      --set storegateway.enabled=true \
      --set bucketConfig.secretName=thanos-bucket-config
    ```

7. **Install Thanos Ruler**
    ```bash
    helm install thanos-ruler bitnami/thanos \
      --set ruler.enabled=true \
      --set ruler.alertmanagers.url=http://prometheus-alertmanager.default.svc.cluster.local:9093
    ```

8. **Verify Installation**
    ```bash
    helm list
    kubectl get pods
    ```

9. **Access Services**
    ```bash
    kubectl port-forward svc/prometheus-server 9090:9090
    kubectl port-forward svc/thanos-query 9091:9090
    ```

10. **Clean Up**
    ```bash
    helm uninstall prometheus
    helm uninstall thanos
    ```

You can deploy Prometheus and Thanos on any of the following Kubernetes cluster environments:

- **Minikube**: Ideal for local development and testing.
- **Google Kubernetes Engine (GKE)**: Managed Kubernetes service by Google Cloud Platform.
- **Amazon Elastic Kubernetes Service (EKS)**: Managed Kubernetes service by Amazon Web Services.
- **Azure Kubernetes Service (AKS)**: Managed Kubernetes service by Microsoft Azure.
## Exposing Services

To access Prometheus and Thanos from outside the cluster, you need to configure an Ingress or use LoadBalancer services.

### Option 1: Configure Ingress

Set up Ingress resources to route external traffic to Prometheus and Thanos services.

**Note**: Ensure that an Ingress Controller (e.g., NGINX Ingress Controller) is installed in your cluster.

Create an Ingress resource for Prometheus:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: prometheus-ingress
    namespace: monitoring
spec:
    rules:
        - host: prometheus.example.com
            http:
                paths:
                    - path: /
                        pathType: Prefix
                        backend:
                            service:
                                name: prometheus-server
                                port:
                                    number: 9090
```

Repeat similar steps for Thanos components as needed.

### Option 2: Use LoadBalancer Services

Alternatively, set the services to type `LoadBalancer` during Helm installation:

```bash
helm install prometheus bitnami/prometheus \
        --namespace monitoring \
        --set server.service.type=LoadBalancer \
        --set alertmanager.enabled=false \
        --set pushgateway.enabled=false
```

### ConfigMap for Proxy Configuration

If you are deploying a proxy (e.g., NGINX, HAProxy) in front of Prometheus and Thanos, you may need a ConfigMap to store the proxy configuration.

Create a ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: proxy-config
    namespace: monitoring
data:
    proxy.conf: |
        # Proxy configuration settings
```

Mount this ConfigMap in your proxy deployment to apply the configuration.
