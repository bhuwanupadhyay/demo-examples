# Kind Cluster with Nginx Ingress Controller

## Create Kind Cluster

```bash
kind create cluster --name c1 --config kind-cluster.yaml 
```

## To see cluster info

```bash
kubectl cluster-info --context kind-c1
```

## Use Kind Cluster k8s Context

```bash
kubectl config use-context kind-c1
```

## Install MetalLB on Kind

```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/master/manifests/namespace.yaml
```

```bash
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```

```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/master/manifests/metallb.yaml
```

```bash
kubectl get pod -n metallb-system
```

```bash
docker network inspect -f '{{.IPAM.Config}}' kind
```

Output Example: `[{172.18.0.0/16  172.18.0.1 map[]} {fc00:f853:ccd:e793::/64   map[]}]`

Here address pool for kind is `172.18.0.0/16`, consider if you want to configure 30 IPs starting from `172.18.255.200`. Then,

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 172.18.255.200-172.18.255.230
EOF
```

## Delete Kind Cluster

```bash
kind delete cluster --name c1
```