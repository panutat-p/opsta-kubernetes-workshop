### Create Kubernetes Cluster on Google Cloud

* Enable container security API
* Enable GKE API
* Download gke-gcloud-auth-plugin

```bash
gcloud components install gke-gcloud-auth-plugin
gke-gcloud-auth-plugin --version 

export USE_GKE_GCLOUD_AUTH_PLUGIN=True

# Create GKE Cluster
gcloud container --project "$PROJECT_ID" clusters create "$K8S_NAME" \
  --zone "$K8S_ZONE" \
  --release-channel "rapid" \
  --machine-type "e2-medium" \
  --enable-ip-alias \
  --image-type "COS_CONTAINERD" \
  --disk-size "100" \
  --num-nodes "2" \
  --network "default" \
  --subnetwork "default" --preemptible

# Get kubeconfig from GKE
gcloud container clusters get-credentials $K8S_NAME --project $PROJECT_ID --zone $K8S_ZONE

# Recheck current context
kubectl config get-contexts
kubectl config current-context
kubectl config use-context gke_opsta-kubernetes-workshop_asia-southeast1-b_skooldio-k8s

# Get cluster info on GKE
kubectl cluster-info
kubectl get pod --all-namespaces
```

# Inspect
[Go to GKE console](https://console.cloud.google.com/kubernetes/workload_/gcloud/asia-southeast1-b/skooldio-k8s?project=opsta-kubernetes-workshop)

### Prepare ingress controller and map domain

```bash
# Add Nginx Helm Repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

# Deploy Nginx Controller on GKE
helm install ingress-nginx ingress-nginx/ingress-nginx --namespace kube-system

# You need to wait for few minutes before you will get the EXTERNAL-IP
# Get Controller IP Address
echo $(kubectl --namespace kube-system get services ingress-nginx-controller \
  --output jsonpath='{.status.loadBalancer.ingress[0].ip}')
```

* Map your domain with IP Address above
