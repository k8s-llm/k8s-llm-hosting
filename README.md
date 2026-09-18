# Local LLM

Idea is to create a way to host LLM + agents + Viewer on k8s

## Motivation

I want to store LLM models in a k8s cluster so they can be consumed from agents or event an UI, always locally, without accessing to a remote based API. 

This does not mean that models should not browse the internet (do they? or would the agent do so?), but they should not inform anyone what is happening with them.

## Proposed architecture

There are many things simultaneously, but I'll try to explain it in a sequence.

## Models
Models can be downloaded to a shared volume using a "singleton" pod (or even eventual one, created and destroying when used) that pulls them from the internet. Any ollama pod will only read the model and not block or try to write the filesystem. If this is possible, how can I tell it "store the model in this directory"?

Then my representation of an operative model is an ollama/llama.cpp/others deployment mounting that volume and using only one of those models.

Those deployments will have an hpa definition that will scale based on a business metric, like connections, tokens per connection, or something like that.
Of course, those will have a service that will let traffic. Should we have stickiness? Or context will come and go from the calls?
How would I manage them? Well, each time we want to support a new model, I would:
a) install a helm implementation to support the model
b) have a general CRD and create new instances of the CRD when you want to support a new model: i.e. Kind: mymodel. An operator should receive a trigger when its created and propose the implementation.

## Networking
In front of that set of deployments I'd like to have a router; but this router should not decide which model to use, but depending on a header or something in the messages, would redirect traffic.
Why? All agents using ollama should access the same endpoint, but indicating a model (like it would do to any ollama implementation). The difference here is that each ollama will work with a single model. If not, pods will be serving many of them, sometimes simultaneously. Might not be scalable.

Do I have a way to identify ollama protocol way of choosing the model?
The way to identify such traffic is by inspecting the message; payload contains the model in a json object. This can be implemented with tinyLLM Proxy, or a custom middleware in traefik or even using envoy as gateway api implementation.


But: 

Otari would be an option for this work; can you indicate the model you'd like to use to otari?

I think otari would replace all this functionality.

## UI
Open WebUI seems nice to at least have an interface to test the models. 
But we can also leverage its capabilities by using some agents behind and run what we need

## Agents
Hermes? OpenClaw? Other agents to host tasks in a company? 

## Monitoring
Of course, we need to monitor everything. Sometimes logs might be verbose enough to try not to publish them. So we should have our own suite.
Grafana/Loki sounds nice.
Have in mind that grafana should get some business metrics in order to feed hpa (Not sure if just cpu + mem will be enough)

## Model routing
Should we need it? Sure ... I'd trust Otari, would it run ok.


## Installing a local test

1) Install Gateway API dependencies

kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.6.1/standard-install.yaml

2) Install Traefik 

In routing/traefik directory:
```bash
helm repo add traefik https://traefik.github.io/charts
helm repo update
helm install traefik traefik/traefik -f values.yaml -n traefik --create-namespace
```

4) Create Volume Claim

Execute:
```bash
kubectl apply -f models/volume.yaml
```

3) Download Models

Execute:
```bash
kubectl apply -f models/ollama/different-ollamas.yaml 
```

Then for each one of the pods running, download the respective model. i.e.:

```bash
kubectl exec ollama-qwen2.5-coder-1.5b-xxxxxxxxxx-xxxxx -- ollama pull qwen2.5-coder:1.5b
kubectl exec ollama-qwnen3-0.6b-xxxxxxxxxx-xxxxx -- ollama pull qwen3:0.6b
```

After downloading, you can remove the deployments. This can be done by pods that finish download and are finished.

Removal can be done by using:
```bash
kubectl delete -f models/ollama/different-ollamas.yaml
```

4) Install Model CRs

Install CRDs using:
```bash
kubectl apply -f https://raw.githubusercontent.com/k8s-llm/k8s-model-operator/main/dist/install.yaml
```

Then run 
```bash
kubectl apply -f models/ollama/llmmodel_v1alpha1_model.yaml
```

5) Install Routes
Run:

```bash
kubectl apply -f routing/routes.yaml
```

6) Install OpenWebUI

```bash
helm repo add open-webui https://open-webui.github.io/helm-charts
helm repo update
helm install openwebui open-webui/open-webui -f open-webui/values.yaml
```

In order to test installation, you can create a port-forward to open-webui server:

```bash
kubectl port-forward svc/openwebui-open-webui 8080:80
```

And connect via browser to http://127.0.0.1:8080

## Software used
Ollama as a container
open-webui as a web viewer



#ollama-k8s

this project aims at configuring ollama + kubectl-ai client in a kubernetes cluster in order to have an LLM operating the k8s cluster.

Will kubectl be able to run inside a pod, pointing at the same cluster?

Download model:
kubectl exec ollama-59f8f464dd-sw5jw -- ollama pull qwen3:0.6B

kubectl-ai --llm-provider ollama --model qwen3:0.6B --enable-tool-use-shim

