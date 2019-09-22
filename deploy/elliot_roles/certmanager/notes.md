# Installation option 1 for cert-manager

# Create a namespace to run cert-manager in
kubectl create namespace cert-manager

# Disable resource validation on the cert-manager namespace
kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true

# first check kubernetes version and ensure it's 1.13 onwards before running next command

# Install the CustomResourceDefinitions and cert-manager itself
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v0.10.0/cert-manager.yaml

# elevate your permissions
kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole=cluster-admin \
  --user=$(gcloud config get-value core/account)






# Installation option 2 for cert-manager
# this option is preferred and we use Helm

First install helm and tiller (ensuring same version is great)

# Install the CustomResourceDefinition resources separately
kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.10/deploy/manifests/00-crds.yaml

# Create the namespace for cert-manager
kubectl create namespace cert-manager

# Label the cert-manager namespace to disable resource validation
kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true

# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

# Update your local Helm chart repository cache
helm repo update

# Install the cert-manager Helm chart
helm install \
  --name cert-manager \
  --namespace cert-manager \
  --version v0.10.0 \
  jetstack/cert-manager

# verify cert-manager is running
kubectl get pods --namespace cert-manager