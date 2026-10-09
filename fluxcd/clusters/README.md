# FluxCD clusters

## Installing local cluster


``` bash
kubectl apply --server-side -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.6.1/standard-install.yaml
helm install -n flux-system --create-namespace flux oci://ghcr.io/fluxcd-community/charts/flux2

kubectl apply -f https://raw.githubusercontent.com/k8s-llm/k8s-model-operator/image-reference/dist/install.yaml
kubectl create ns traefik
kubectl create ns models
kubectl apply -f ./fluxcd/clusters/local-kustomization.yaml

```

In order to download models for this example, you should create pods that attach the created volume and download models (as done in github action for testing)

i.e.: 

```bash
kubectl create -f models/ollama/different-ollamas.yaml -n models
kubectl exec $(kubectl get pod -l app=ollama-qwen2.5-coder-1.5b -o jsonpath='{.items[0].metadata.name}') -- ollama pull qwen2.5-coder:1.5b
kubectl exec $(kubectl get pod -l app=ollama-qwnen3-0.6b jsonpath='{.items[0].metadata.name}') -- ollama pull qwen3:0.6b
kubectl delete -f models/ollama/different-ollamas.yaml -n models
```


## FluxCD CLI

```bash
curl -s https://fluxcd.io/install.sh | sudo bash
```
