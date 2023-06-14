# qa-testkube

Teskube deployment scripts

## Create Git Repository

```sh
export WORKSPACE_NAMESPACE=kommander-default-workspace
kubectl apply -f - <<EOF
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: GitRepository
metadata:
  name: qa-testkube
  namespace: ${WORKSPACE_NAMESPACE}
  labels:
    kommander.d2iq.io/gitapps-gitrepository-type: dkp
    kommander.d2iq.io/gitrepository-type: catalog
spec:
  interval: 1m0s
  ref:
    branch: main
  timeout: 20s
  url: https://github.com/rodrigodelmonte/qa-testkube
EOF
```

## Enable Testkube Application using the CLI

```sh
export WORKSPACE_NAMESPACE=kommander-default-workspace
kubectl apply -f - <<EOF
apiVersion: apps.kommander.d2iq.io/v1alpha3
kind: AppDeployment
metadata:
  name: testkube
  namespace: ${WORKSPACE_NAMESPACE}
spec:
  appRef:
    name: testkube-1.12.16
    kind: App
  clusterSelector:
    matchExpressions:
    - key: kommander.d2iq.io/cluster-name
      operator: In
      values:
      - host-cluster
EOF
```
