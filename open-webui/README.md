# open-webui


I want to install openwebui without ollama included in it, since I want to have control on the model servers I use locally.


Helm documentation: https://github.com/open-webui/helm-charts/blob/main/charts/open-webui/README.md


https://docs.openwebui.com/getting-started/quick-start#helm-steps

## Route configuration 

Check that routes to models are configured in values.yaml. In our case, we are creating external name Services, so we have different urls for each model implemented. Configuring endpoints with equal url is not supported via values.yaml, so this is a great solution.

```
helm repo add open-webui https://open-webui.github.io/helm-charts
helm repo update
helm install openwebui open-webui/open-webui -f values.yaml






```
outputs:
```
NAME: openwebui
LAST DEPLOYED: Thu Aug 13 17:13:14 2026
NAMESPACE: default
STATUS: deployed
REVISION: 1
DESCRIPTION: Install complete
NOTES:
🎉 Welcome to Open WebUI!!
 ██████╗ ██████╗ ███████╗███╗   ██╗    ██╗    ██╗███████╗██████╗ ██╗   ██╗██╗
██╔═══██╗██╔══██╗██╔════╝████╗  ██║    ██║    ██║██╔════╝██╔══██╗██║   ██║██║
██║   ██║██████╔╝█████╗  ██╔██╗ ██║    ██║ █╗ ██║█████╗  ██████╔╝██║   ██║██║
██║   ██║██╔═══╝ ██╔══╝  ██║╚██╗██║    ██║███╗██║██╔══╝  ██╔══██╗██║   ██║██║
╚██████╔╝██║     ███████╗██║ ╚████║    ╚███╔███╔╝███████╗██████╔╝╚██████╔╝██║
 ╚═════╝ ╚═╝     ╚══════╝╚═╝  ╚═══╝     ╚══╝╚══╝ ╚══════╝╚═════╝  ╚═════╝ ╚═╝

v0.11.0 - building the best open-source AI user interface.
 - Chart Version: v16.0.0
 - Project URL 1: https://www.openwebui.com/
 - Project URL 2: https://github.com/open-webui/open-webui
 - Documentation: https://docs.openwebui.com/
 - Chart URL: https://github.com/open-webui/helm-charts

Open WebUI is a web-based user interface that works with Ollama, OpenAI, Claude 3, Gemini and more.
This interface allows you to easily interact with local AI models.

1. Deployment Information:
  - Chart Name: open-webui
  - Release Name: openwebui
  - Namespace: default

2. Access the Application:
  Access via ClusterIP service:

    export LOCAL_PORT=8080
    export POD_NAME=$(kubectl get pods -n default -l "app.kubernetes.io/component=openwebui-open-webui" -o jsonpath="{.items[0].metadata.name}")
    export CONTAINER_PORT=$(kubectl get pod -n default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
    kubectl -n default port-forward $POD_NAME $LOCAL_PORT:$CONTAINER_PORT
    echo "Visit http://127.0.0.1:$LOCAL_PORT to use your application"

  Then, access the application at: http://127.0.0.1:$LOCAL_PORT or http://localhost:8080

3. Useful Commands:
  - Check deployment status:
      helm status openwebui -n default

  - Get detailed information:
      helm get all openwebui -n default

  - View logs:
      kubectl logs -f statefulset/openwebui-open-webui -n default

4. Cleanup:
  - Uninstall the deployment:
      helm uninstall openwebui -n default
```
#ollama-k8s

this project aims at configuring ollama + kubectl-ai client in a kubernetes cluster in order to have an LLM operating the k8s cluster.

Will kubectl be able to run inside a pod, pointing at the same cluster?

Download model:
kubectl exec ollama-59f8f464dd-sw5jw -- ollama pull qwen3:0.6B

kubectl-ai --llm-provider ollama --model qwen3:0.6B --enable-tool-use-shim

