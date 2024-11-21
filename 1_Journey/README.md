Steps to follow
Start minikube
Check minikube status
create configuration for Prometheus
create config for thanos service:
1. frontend
2. query
3. reciever
Ingress Network will use ngnix as proxy
Config proxy settings
setup ruler and alter manager
Expose remote right prometheus to thanos reciever 

We need the frontend, reciever, ruler and query to communicate via proxy