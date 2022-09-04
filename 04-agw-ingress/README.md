# Install


## 

```azcli
# Create a namespace for your ingress resources
kubectl create namespace ingress

# Add the ingress-nginx repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

# Use Helm to deploy an NGINX ingress controller
helm install nginx-ingress ingress-nginx/ingress-nginx \
    --namespace ingress \
    -f internal-ingress.yaml \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set controller.admissionWebhooks.patch.nodeSelector."beta\.kubernetes\.io/os"=linux
```
It may take a few minutes for the IP address to be assigned.

```
kubectl --namespace ingress get services -o wide -w nginx-ingress-ingress-nginx-controller

# You can verify this by looking at the events for the service.

kubectl --namespace ingress get services -o wide -w nginx-ingress-ingress-nginx-controller

```

## Deploying two test applications

```
kubectl create ns ingress-test
```

