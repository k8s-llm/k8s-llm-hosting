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


---
github-actions-different-ollamas.yaml is a file that contains models to be created by github action that tests an implementation in Github.
In order to run that tests, pushes should me made to branches named: `test/*`, `testing/*` or `github-action-setup`.

## Different models used by OpenWebUI

As a tool to test, we are using OpenWebUI.

It is important to be compliant with it, so we are consuming ollama managed models by using OpenAI inferface.

In order to have OpenWebUI uniquely identifying models, we are using External Name services to create alternatve namings for traefik endpoint. This way each model is exposed by using a different url (and a specific header, since routing was based on it)

As external name service were implemented later, maybe using header is obsolete and we can route based on host.

