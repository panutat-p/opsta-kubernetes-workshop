## Label and Selector

```bash
# Create new apache deployment
kubectl create deployment apache --image=httpd:2.4-alpine
# Scale apache deployment to 3 replicas
kubectl scale deployment apache --replicas=3
# See change
kubectl get deployment,pod
# See the label and selector
kubectl describe deployments nginx
kubectl describe service nginx
# See the label
kubectl describe deployments apache
# Set service nginx to select apache deployment label instead
kubectl set selector service nginx 'app=apache'
# Revert selector back
kubectl set selector service nginx 'app=nginx'
```

## Kubernetes Utilities Commands

```bash
# Show pod log
kubectl get pod
kubectl logs apache-5d94cf486d-65m4b -f
# Enter inside container
kubectl get service
kubectl exec -it apache-5d94cf486d-65m4b -- sh
ping nginx
exit
# View node information
kubectl get nodes
kubectl describe nodes gke-training-default-pool-115e6de5-shkw
```

## Clear Everything

```bash
kubectl delete deployment nginx apache
kubectl delete service nginx
kubectl get deployment,pod
```
