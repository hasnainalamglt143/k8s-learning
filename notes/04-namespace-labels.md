# 05 - Labels, Selectors, Namespaces

## Labels
- key-value tags on any object; an object can have many
- Used for SELECTING: ReplicaSet → pods, Service → pods, kubectl filtering
- Common keys: app, tier, env, version
- Annotations = notes only, never used for selection (e.g. change-cause)

## Selectors
- Equality: app=web, env!=prod, app=web,env=dev (comma = AND)
- Set: "env in (dev,staging)", "env notin (prod)", tier (exists), !tier
- YAML: matchLabels (common) / matchExpressions (set-based)

## Label commands
kubectl get pods --show-labels
kubectl get pods -l env=dev           # filter
kubectl get pods -L env,tier          # labels as columns
kubectl label pod X key=val           # add
kubectl label pod X key=val --overwrite
kubectl label pod X key-              # remove
kubectl delete pods -l app=shop

## Key experiment
Relabel a Deployment pod (app=web → app=debug):
→ RS sees only 2 matching → creates a new one
→ old pod keeps running as orphan, removed from Service endpoints
Use: isolate a bad pod from traffic for debugging

## Namespaces
- Virtual folders in a cluster; names unique per namespace
- Give: name isolation, organization, RBAC scope, resource quotas
- Do NOT give network isolation by default → need NetworkPolicy
- Built-in: default, kube-system, kube-public, kube-node-lease
- Namespaced: pods, deploy, svc, configmap, secret, pvc
- Cluster-wide: nodes, namespaces, PVs, storageclasses
- Deleting a namespace deletes everything in it

## Namespace commands
kubectl get ns
kubectl create namespace dev
kubectl get pods -n dev | -A
metadata.namespace: dev            # in YAML (preferred)
kubectl config set-context --current --namespace=dev
kubectl api-resources --namespaced=true|false

## Cross-namespace DNS
web          → same namespace
web.dev      → service web in namespace dev
web.dev.svc.cluster.local → full name