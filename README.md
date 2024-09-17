kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
kubectl expose deployment argo-cd-argocd-server -n argocd --type=NodePort --dry-run=client -o yaml > kubernetes-argocd-nodeport.yaml
kubectl apply -f kubernetes-argocd-nodeport.yaml
