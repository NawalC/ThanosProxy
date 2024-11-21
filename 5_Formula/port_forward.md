# Port Forwarding Prometheus

To access Prometheus from your local machine, you can use the `kubectl port-forward` command. This will forward a port from your local machine to a port on the Prometheus service running in your Kubernetes cluster.

## Command

```sh
kubectl port-forward svc/prometheus-server 9090:9090
```

This command forwards port 9090 on your local machine to port 9090 on the Prometheus service.

## Steps

1. Ensure you have `kubectl` installed and configured to access your Kubernetes cluster.
2. Run the port-forward command:
    ```sh
    kubectl port-forward svc/prometheus-server 9090:9090
    ```
3. Open your web browser and navigate to `http://localhost:9090` to access the Prometheus web interface.

## Notes

- The `svc/prometheus-server` refers to the Prometheus service in your Kubernetes cluster. Ensure that the service name matches the actual service name in your cluster.
- Port forwarding will run in the foreground. You can stop it by pressing `Ctrl+C`.

This setup allows you to access Prometheus locally without exposing it to the public internet.