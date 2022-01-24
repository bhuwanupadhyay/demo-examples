# Kind Cluster with Nginx Ingress Controller

## Create Kind Cluster

```bash
kind create cluster --name c1 --config kind-cluster.yaml 
```

## Install LoadBalancer

<https://kind.sigs.k8s.io/docs/user/loadbalancer/>

## Install Nginx Ingress Controller

```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

## Expose Application behind Nginx Ingress Controller

Deploy application

```bash
kubectl apply -f foo-bar.yaml
```

Verify application

```bash
curl -v -H "Host:foo-bar.kind.local" http://172.19.255.200/foo

curl -v -H "Host:foo-bar.kind.local" http://172.19.255.200/bar
```

## Delete Kind Cluster

```bash
kind delete cluster --name c1
```

# References
- https://kind.sigs.k8s.io/docs/user/loadbalancer/