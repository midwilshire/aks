# Deploy Basic Example

## Run demo applications

```azcli
kubectl apply -f azure-vote.yaml
```

## Test the application

```azcli
kubectl get service azure-vote-front --watch
```
