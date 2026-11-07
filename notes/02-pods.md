# 02 - Pods

## What
- Smallest deployable unit in K8s; wraps 1+ containers
- Containers in one pod share IP (talk via localhost) and storage, run on same node
- Usually 1 pod = 1 main container; extra helper = "sidecar"
- Pods are EPHEMERAL: new pod = new IP. Don't rely on a specific pod.
- Bare pods are NOT self-healing → use Deployments in real work

## YAML rules
- spaces only, 2 per level, space after colon
- `- ` = list item
- Every manifest: apiVersion, kind, metadata, spec

## Pod YAML
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx
  labels:
    app: web
spec:
  containers:
    - name: nginx
      image: nginx:1.25
      ports:
        - containerPort: 80

## Commands
| Command | Purpose |
|---|---|
| `kubectl apply -f file.yaml` | create/update |
| `kubectl get pods [-o wide] [-w]` | list (with IP/node) (watch) |
| `kubectl describe pod NAME` | details + Events (debug first) |
| `kubectl logs NAME [--previous] [-c container]` | app logs |
| `kubectl exec -it NAME [-c container] -- sh` | shell inside |
| `kubectl port-forward pod/NAME 8080:80` | test access from laptop |
| `kubectl delete pod NAME` | delete |
| `kubectl run x --image=nginx --dry-run=client -o yaml` | generate YAML |

## Statuses
Pending | ContainerCreating | Running | Completed
ImagePullBackOff → wrong image/tag/registry auth
CrashLoopBackOff → app crashing; check `logs --previous`

## Debug order
get → describe (Events) → logs (--previous) → exec