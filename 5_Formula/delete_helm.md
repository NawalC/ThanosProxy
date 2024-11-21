# Deleting and Redeploying Prometheus with Port Forwarding

To delete the existing Prometheus deployment and redeploy it with port forwarding, follow these steps:

## Step 1: Delete Existing Prometheus Deployment

First, delete the existing Prometheus deployment using Helm:

```sh
helm uninstall prometheus --namespace monitoring
```

## Step 2: Redeploy Prometheus

Next, redeploy Prometheus using the Bitnami Helm chart and set the service type to `LoadBalancer`:

```sh
helm install prometheus bitnami/prometheus \
    --namespace monitoring \
    --set server.service.type=LoadBalancer \
    --set alertmanager.enabled=true \
    --set server.service.nodePort=9090 
```

## Step 3: Port Forward to Prometheus

After redeploying Prometheus, use the `kubectl port-forward` command to forward port 9090 on your local machine to port 9090 on the Prometheus service:

```sh
kubectl port-forward svc/prometheus-server 9090:9090 --namespace monitoring
```

This command will allow you to access the Prometheus web interface locally at `http://localhost:9090`.

## Notes

- Ensure you have `kubectl` installed and configured to access your Kubernetes cluster.
- The `svc/prometheus-server` refers to the Prometheus service in your Kubernetes cluster. Ensure that the service name matches the actual service name in your cluster.
- Port forwarding will run in the foreground. You can stop it by pressing `Ctrl+C`.

This setup allows you to access Prometheus locally without exposing it to the public internet.