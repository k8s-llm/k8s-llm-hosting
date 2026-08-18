How I'm creating a local kind cluster to use nvidia gpu:



`nvkind cluster create --config-template=one-node-per-gpu.yaml --name k8s-models`

This uses an nvkind example owned by nvidia, and adds association to a local folder, so worker nodes contain it.

`kubectl create -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v0.17.1/deployments/static/nvidia-device-plugin.yml`

```
export KIND_CLUSTER_NAME=k8s-models
helm upgrade -i \
    --kube-context=kind-${KIND_CLUSTER_NAME} \
    --namespace gpu-operator \
    --create-namespace \
    --wait \
    nvidia-gpu-operator nvidia/gpu-operator \
    --set cdi.enabled=true \
    --set driver.enabled=false \
    --set toolkit.enabled=false \
    --set operator.runtimeClass=nvidia
```

