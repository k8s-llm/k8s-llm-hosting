# Models

This directory contains definition for persistent volume and volume claim to be used in our pods. In this case, specific to using kind mounted volume sharing a system directory.


## How will we download new models?

A new CRD of type llmmodel would be created.

Existance of a new object of this kind will trigger
a) running a job of an image of the model manager selected, assuming a volume to place the model there. Job would run a command like "ollama pull modelname" and end.
b) creation of a new deployment implementing one of the model managers selected

Both job and deployment should have an envvar set named 
        - name: OLLAMA_MODELS
          value: /data/models/ollama/{model}

So any reader will only see its own model. The idea is that each instance of ollama (or whatever manager) would read only the required model.

job (a) may check if the model already exists and avoid duplication.
