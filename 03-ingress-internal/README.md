# Deploy Ingress Internal Example

## Prerequisites

```azcli
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

## Installing NGINX Ingress

Create a namespace in your cluster.

```azcli
kubectl create namespace ingress-nginx
```

Run below command to install. This will create a load balancer resource with a public IP, in the AKS shadow resource group.

```azcli
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
  --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
  --set controller.service.type=LoadBalancer \
  --set controller.service.externalTrafficPolicy=Local
  --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-internal"="true"
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

## Test an internal IP address

To test the routes for the ingress controller using an internal IP, create a test pod and attach a terminal session to it:

```azcli
kubectl run -it --rm aks-ingress-test --image=mcr.microsoft.com/dotnet/runtime-deps:6.0 --namespace ingress-basic
```
Install curl in the pod using apt-get:

```azcli
apt-get update && apt-get install -y curl
```

Now access the address of your Kubernetes ingress controller using curl, such as http://10.224.0.42. Provide your own internal IP address specified when you deployed the ingress controller.

```azcli
curl -L http://10.224.0.42
```
