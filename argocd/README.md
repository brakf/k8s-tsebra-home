Install Argo CD via:

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

Create Ingress for Argo CD; accessible via traefik
