# 00 - Setup

## Tools
- **minikube**: runs a single-node Kubernetes cluster locally (inside Docker here)
- **kubectl**: CLI that talks to the cluster's API server
- **Docker**: container runtime / minikube driver

## Commands
| Command | Purpose |
|---|---|
| `minikube start --driver=docker` | create/start local cluster |
| `minikube status` | check host, kubelet, apiserver |
| `minikube stop / delete` | pause / destroy cluster |
| `kubectl cluster-info` | API server address |
| `kubectl get nodes` | list nodes |
| `kubectl get pods -A` | pods in all namespaces |

## kubeconfig
- kubectl reads `~/.kube/config` to know which cluster to talk to
- `kubectl config current-context` → shows `minikube`
- In production you switch context to EKS/GKE/AKS; the same kubectl commands work

## Local vs Production
- minikube = 1 node, control plane + worker on same machine, for learning/dev
- production = multi-node, managed control plane, HA