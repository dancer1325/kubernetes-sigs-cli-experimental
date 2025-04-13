---
title: "apply"
linkTitle: "apply"
weight: 1
type: docs
description: >
    Using apply Command
---

## apply -- via -- `YAML files`

* can be applied to
  * Resource Config files or
  * directories

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: the-deployment
spec:
  replicas: 5
  template:
    containers:
      - name: the-container
        image: registry/container:latest
```

```bash
# Apply the Resource Config
kubectl apply -f deployment.yaml
```

## apply -- with -- `Kustomize files`

* if you want to apply | 
  * directories / contain `kustomization.yaml` -> recommended to use `-k`
  * raw ResourceConfig files -> recommended to use `-f`

```yaml
# kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

# list of Resource Config to be Applied
resources:
- deployment.yaml

# namespace to deploy all Resources to
namespace: default

# labels added to all Resources
commonLabels:
  app: example
  env: test
```

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: the-deployment
spec:
  replicas: 5
  template:
    containers:
      - name: the-container
        image: registry/container:latest
```

```bash
# Apply the Resource Config
kubectl apply -k .

# View the Resources
kubectl get -k .
```
