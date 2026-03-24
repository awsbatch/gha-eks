# Command related to this practical
-------------------------------------------
## Download eksctl Binary
```
curl -sL https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Windows_amd64.zip -o eksctl.zip
```
```
unzip eksctl.zip
```
```
mv eksctl.exe /c/Windows/System32/
```
### Verify Installation
```
eksctl version
```

## EKS Cluster Creation
```
eksctl create cluster \
  --name my-cluster \
  --region ap-south-1 \
  --nodegroup-name my-nodes \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3 \
  --managed
```
## Delete EKS Cluster
```
eksctl delete cluster --name my-cluster --region ap-south-1
```


# Install Argocd on EKS Cluster
### Create Namespace
```
kubectl create namespace argocd
```
### Install ArgoCD
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
### Wait for Pods
```
kubectl get pods -n argocd
```
> [!IMPORTANT]
> ### Expose ArgoCD UI (Important)

```
kubectl patch svc argocd-server -n argocd \
  -p '{"spec": {"type": "LoadBalancer"}}'
```

### Get Admin Password

```
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d
```

### Install yq on Linux / Git Bash

```
sudo wget https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 -O /usr/local/bin/yq
sudo chmod +x /usr/local/bin/yq
```

### Commit and push yaml file content use this code only gha yaml file

```
      - name: Commit & Push Changes
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions@github.com"
          git add k8s/pyapp-deployment.yml
          git commit -m "Update image tag to ${{ env.SHORT_SHA }}" || echo "No changes"
          git push
```
