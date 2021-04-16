# argocd-helmchart
A repository to test ArgoCD with Helm

# Install ArgoCD

```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.0.0/manifests/install.yaml
```

## Get pwd

```shell
ARGO_PWD=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
# Set in nodePort
kubectl patch svc argocd-server -n argocd -p '{"spec": { "type": "NodePort", "ports": [ { "nodePort": 31550, "port": 80, "protocol": "TCP", "targetPort": 8080 } ] } }'
argocd login --username admin --password $ARGO_PWD localhost:31550 --insecure
# argocd account update-password
```
