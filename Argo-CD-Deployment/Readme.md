Argo-CD installation in minikube

**1. To download the Argocd code**

```
curl -L \
  https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml \
  -o argocd-install.yaml
```

**2.Create argocd namespace**
```
kubectl create ns argocd
```
**3.Install Argo CD using your YAML**
```
kubectl apply -n argocd \
  --server-side \
  --force-conflicts \
  -f argocd-install.yaml

```
**4.Verify pods in argocd namespace**
```
kubectl get po -n argocd
```
**5.Access the Argo CD UI from Minikube**
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
**Open in local**
```
https://localhost:8080
```
**6.Get the initial admin password**
```
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d
```
**Login Credential**
```
Username: admin
Password: <the password above>
```

**Complete Script**
```
minikube start

kubectl create namespace argocd

curl -L \
  https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml \
  -o argocd-install.yaml

kubectl apply -n argocd \
  --server-side \
  --force-conflicts \
  -f argocd-install.yaml

kubectl get pods -n argocd

kubectl port-forward svc/argocd-server -n argocd 8080:443
```
Then Vist 
```
https://localhost:8080
```