

helm repo add nvdp https://nvidia.github.io/k8s-device-plugin
helm repo update

helm install nvdp nvdp/k8s-device-plugin \
  --namespace kube-system \
  --set compatWithCRI=true

kubectl create -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v0.17.1/deployments/static/nvidia-device-plugin.yml
------------------




https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html
https://github.com/nvidia/nvkind

----

I think the first is not enough:

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


