# Deploy Ingress Example

## Prerequisites

```azcli
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

## Installing NGINX Ingress

Create a namespace in your cluster.

```azcli
kubectl create namespace ingress-nginx
kubectl create namespace ingress-basic
```

Run below command to install. This will create a load balancer resource with a public IP, in the AKS shadow resource group.

```azcli
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
  --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
  --set controller.service.type=LoadBalancer \
  --set controller.service.externalTrafficPolicy=Local
```

## Verify

```azcli
kubectl describe service ingress-nginx-controller --namespace ingress-nginx
```

## Run demo applications

```azcli
kubectl apply -f aks-helloworld-one.yaml --namespace ingress-basic
kubectl apply -f aks-helloworld-two.yaml --namespace ingress-basic
```

## Create an ingress route

```azcli
kubectl apply -f hello-world-ingress.yaml --namespace ingress-basic
```

## Check the website

http//ipaddress/hello-world-two