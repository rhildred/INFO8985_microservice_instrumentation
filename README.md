# k3d-helm
run kubernetes cluster in codespace

TLDR;

```bash
pip install -r requirements.txt
ansible-playbook playbook.yml

```

I did struggle with actually seeing the service outside of the dev container. If the service port is 80 and you want to access on 8080, you can run:

```bash
nohup kubectl port-forward svc/< what you named service > 8080:80  2>&1 &
```

This repo makes a persistent volume from the storage folder. Consume it like this:

```yml
# Additional volumes on the output Deployment definition.
volumes: [ { "name": "my-volume", "persistentVolumeClaim": { "claimName": "my-pvc" }}]

# Additional volumeMounts on the output Deployment definition.
volumeMounts: [ { "mountPath": "/usr/share/nginx/html", "name": "my-volume" }]
```

To delete a cluster (and start over):

```bash
k3d cluster delete local-k8s
```

Use this as a template and then add helm charts to the playbook.yml as required.
