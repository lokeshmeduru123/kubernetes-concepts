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

**To delete the resources**
```
kubectl delete -n argocd -f argocd-install.yaml
```

**To expose the argocd using nodeport in minikube**
```
kubectl apply -f argocd-install-nodeport.yaml -n argocd
```

** verify the service**
```
kubectl get svc -n argocd
```
Below is the sample output 
```
lokeshmeduru@Lokeshs-MacBook-Pro Argo-CD-Deployment % kubectl get svc -n argocd
NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   10.106.198.6     <none>        7000/TCP,8080/TCP            113s
argocd-dex-server                         ClusterIP   10.96.227.25     <none>        5556/TCP,5557/TCP,5558/TCP   113s
argocd-metrics                            ClusterIP   10.109.249.68    <none>        8082/TCP                     113s
argocd-notifications-controller-metrics   ClusterIP   10.109.210.184   <none>        9001/TCP                     113s
argocd-redis                              ClusterIP   10.102.234.28    <none>        6379/TCP                     113s
argocd-repo-server                        ClusterIP   10.107.27.172    <none>        8081/TCP,8084/TCP            113s
argocd-server                             NodePort    10.100.111.164   <none>        80:30080/TCP,443:30443/TCP   113s
argocd-server-metrics                     ClusterIP   10.99.179.103    <none>        8083/TCP                     113s
lokeshmeduru@Lokeshs-MacBook-Pro Argo-CD-Deployment % 
```