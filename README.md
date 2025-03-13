# INFO8985 Microservice instrumentation
run kubernetes cluster with signoz and log, trace and meter to it from knative func

TLDR;

```bash
pip install -r requirements.txt
ansible-playbook playbook.yml

```

Run `kubectl get pods` to see that the pods are all running or completed. Then you need to forward the ports:

```bash
nohup kubectl port-forward svc/my-signoz-frontend 3301:3301  2>&1 &
nohup kubectl port-forward svc/my-signoz-otel-collector 4317:4317  2>&1 &
```

You can run app.py to see your metrics, logs and traces in signoz. 

```bash
OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true OTEL_EXPORTER_OTLP_ENDPOINT=localhost:4317 OTEL_SERVICE_NAME=io.github.rhildred.INFO8985_microservice_instrumentation OTEL_EXPORTER_OTLP_INSECURE=true opentelemetry-instrument --traces_exporter otlp --metrics_exporter otlp --logs_exporter otlp python app.py
```

All of the apps are reachable in your browser from the globe on the ports tab.

To delete the cluster (and start over):

```bash
k3d cluster delete local-k8s
```

Use this as a template and:

1. use [this article](https://knative.dev/docs/functions/install-func/) to install func. and create a python hello world.
2. use the example in app.py to instrument the code created with func
3. deploy the function on the cluster

Next week we will have a chance to understand the logging in signoz.
