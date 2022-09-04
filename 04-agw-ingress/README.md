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

## Run demo applications

```azcli
kubectl apply -f aks-helloworld-one.yaml --namespace ingress-test
kubectl apply -f aks-helloworld-two.yaml --namespace ingress-test
```

## Create an ingress route

```azcli
kubectl apply -f hello-world-ingress.yaml --namespace ingress-test
```

## Testing
Now, let’s launch a pod to validate the configuration.

```azcli
kubectl run -it --rm aks-ingress-test --image=debian --namespace ingress-test
```

## Update
```
apt-get update && apt-get install -y curl

curl -L -H "Host: mysub.mydomain.com" http://10.1.0.200

```

## Prepare 

```azcli
az network public-ip create -g rg-appg-ingress-test -l eastus2 -n pip-appg-ingress-test --sku Standard

az network application-gateway create --name azappg-appg-ingress-test -g rg-appg-ingress-test -l eastus2 --sku Standard_v2 --public-ip-address pip-appg-ingress-test --vnet-name vnet-ingress-test --subnet appg --servers 10.1.0.200

```

