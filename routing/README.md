This routes are an example of how to create Gateway API httproutes object (implemented by trafeik in my case) and support routing based on a custom header.

Header can be set if you use openAI protocolo to connect to ollama instances. i.e. if using openweb-ui, you can set the headers in json format when using openAI provider.

In my case, I'm using:
http://traefik.traefik.svc.local:80/api 

whith header:

X-Model: qwen3

and 

X-Model: qwen-code

I've created two different openAI providers, pointing at the same endpoint, with different X-Model headers and I see both models available.

