The problem is that the Helm repository 'bitnami' is not added. To fix this, you need to add the 'bitnami' repository using the Helm command.
```
helm repo add bitnami https://charts.bitnami.com/bitnami
```
```
helm repo update
```