```
@NawalC ➜ /workspaces/ThanosProxy (main) $ kubectl get svc -n monitoring
NAME                TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
prometheus-server   ClusterIP      10.96.0.1        <none>        9090/TCP         10m

@NawalC ➜ /workspaces/ThanosProxy (main) $ kubectl port-forward -n monitoring svc/prometheus-server 9090:9090 
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
```