# FluxCD clusters

## Installing local cluster

Not needed: helm repo add traefik https://traefik.github.io/charts

``` bash
kubectl apply --server-side -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.6.1/standard-install.yaml
helm install -n flux-system --create-namespace flux oci://ghcr.io/fluxcd-community/charts/flux2

kubectl apply -f https://raw.githubusercontent.com/k8s-llm/k8s-model-operator/image-reference/dist/install.yaml
kubectl create ns traefik
kubectl create ns models
kubectl apply -f local-kustomization.yaml

```

have to apply  traefik-helm-values-bff4mdhgm4


FluxCD CLI

```bash
curl -s https://fluxcd.io/install.sh | sudo bash
```
