# Command to expose ArgoCD from MicroK8s as NodePort

- https://microk8s.io/docs/addons
- https://argo-cd.readthedocs.io/en/stable/

## Dry run expose, get output as file

```sh
kubectl expose deployment argo-cd-argocd-server -n argocd --type=NodePort --dry-run=client -o yaml > kubernetes-argocd-nodeport.yaml
```
## Change file content

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app.kubernetes.io/component: server
    app.kubernetes.io/instance: argo-cd
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: argocd-server
    app.kubernetes.io/part-of: argocd
    helm.sh/chart: argo-cd-5.34.3
  name: self-argocd-server-nodeport
  namespace: argocd
spec:
  ports:
  - name: port-1
    port: 8080
    protocol: TCP
    targetPort: 8080
    nodePort: 30080
  - name: port-2
    port: 8083
    protocol: TCP
    targetPort: 8083
    nodePort: 30083
  selector:
    app.kubernetes.io/instance: argo-cd
    app.kubernetes.io/name: argocd-server
  type: NodePort
status:
  loadBalancer: {}
```

## Apply kubernetes-argocd-nodeport.yaml
```sh
kubectl apply -f kubernetes-argocd-nodeport.yaml
```

## Get Initial password of `admin`
```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```



