# Helm Installation Error: Namespace Not Found

When attempting to install Prometheus using Helm with the following command:

```sh
helm install prometheus bitnami/prometheus --namespace monitoring --set server.service.type=LoadBalancer --set alertmanager.enabled=true --set pushgateway.enabled=false
```

You might encounter the following error:

```
Error: INSTALLATION FAILED: create: failed to create: namespaces "monitoring" not found
```

## Solution

To resolve this issue, you need to create the `monitoring` namespace before running the Helm install command. You can create the namespace using the following command:

```sh
kubectl create namespace monitoring
```

After creating the namespace, you can proceed with the Helm installation:

```sh
helm install prometheus bitnami/prometheus --namespace monitoring --set server.service.type=LoadBalancer --set alertmanager.enabled=true --set pushgateway.enabled=false
```

This should resolve the namespace not found error and allow the installation to proceed successfully.

fix 
