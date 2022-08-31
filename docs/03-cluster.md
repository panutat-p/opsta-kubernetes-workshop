## Set up your own namespaces

```bash
# Create your own namespaces
kubectl create namespace bookinfo-dev
kubectl create namespace bookinfo-uat
kubectl create namespace bookinfo-prd
# Show your newly created namespace
kubectl get namespaces

# Set default namespace
kubectl config set-context gke_opsta-kubernetes-workshop_asia-southeast1-b_skooldio-k8s \
  --namespace bookinfo-dev
kubectl config get-contexts
```

## Create Pod, Deployment and Service

```bash
# Create nginx deployment
kubectl create deployment nginx --image=nginx
# Show pods
kubectl get pods
# Show deployments
kubectl get deployment
# Show nginx deployment detail
kubectl describe deployment nginx
# Expose service load balancer to nginx deployment port 80
kubectl expose deployment nginx --type LoadBalancer --port 80
# Wait to see public ip to be active and test it
kubectl get services -w
```

## Scale service and change docker image

```bash
# Scale pod to 3 replicas
kubectl scale deployment nginx --replicas=3
# Show deployment and pod status
kubectl get deployment,pod
# Change nginx deployment to use apache instead
kubectl set image deployment nginx nginx=httpd:2.4-alpine --record
# See change
watch -n1 kubectl get pod
kubectl get deployment
kubectl describe deployments nginx
```

## Rollback deployment

```bash
# Show deployment history
kubectl rollout history deployment nginx
# Rollback one version
kubectl rollout undo deployment nginx
# See change
kubectl rollout history deployment nginx
kubectl describe deployment nginx
```
