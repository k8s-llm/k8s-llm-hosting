# Ollama deployments

I created the deployment with this yaml file.

Then downloaded a light qwen model:

`k exec ollama-646dd47cd7-ltdjc -- ollama pull qwen3:0.6b`


---

different-ollamas.yaml is a file that creates deployments to download specific models. 
That can be used as pods and use them only for that purpose, or even create them as jobs that would load ollama, pull the model and exit. Don't even need to run over GPU.

---
llmmodel_v1alpha1_model.yaml file creates two model resources. Those rely on a couple of already downloaded models in the volume we have available. 
services.yaml exposes deployments created by the model resources so we can access them with traefik routing.

We need services created manually now, since model kind does not yet create services and hpa by itself.


