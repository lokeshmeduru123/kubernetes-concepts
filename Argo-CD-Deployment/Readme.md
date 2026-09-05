Argo-CD installation in minikube

**1. To download the Argocd code**

```
curl -L \
  https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml \
  -o argocd-install.yaml
```

**Create argocd namespace**
```
kubectl create ns argocd
```
**Install Argo CD using your YAML**
```
kubectl apply -n argocd \
  --server-side \
  --force-conflicts \
  -f argocd-install.yaml

```