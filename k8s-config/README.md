# Cluster Infrastructure Configuration

This directory contains the setup for third-party controllers and security managers.

## 1. NGINX Ingress Controller
Used to manage external access to the services in the cluster.

```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

## 2. Cert-Manager
Handles automated TLS certificate issuance via Let's Encrypt.
```bash
helm install \
  cert-manager oci://quay.io/jetstack/charts/cert-manager \
  --version v1.19.4 \
  --namespace cert-manager \
  --create-namespace \
  --set crds.enabled=true
```

## 3. Post-Installation
1. Edit `./cert-manager/Issuer.yaml` with your real email
2. Apply cert-manger config:
`kubectl apply -f ./cert-manager/`

