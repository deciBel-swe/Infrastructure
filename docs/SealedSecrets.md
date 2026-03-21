Installation Instructions
> Source: https://github.com/bitnami-labs/sealed-secrets/releases

Cluster-side
```bash
kubectl apply -f https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.36.1/controller.yaml
```

Client Side (Linux):
```bash
curl -OL "https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.36.1/kubeseal-0.36.1-linux-amd64.tar.gz"
tar -xvzf kubeseal-0.36.1-linux-amd64.tar.gz kubeseal
sudo install -m 755 kubeseal /usr/local/bin/kubeseal
```