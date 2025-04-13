---
title: "kustomize create"
linkTitle: "create"
type: docs
weight: 4
description: >
    Create a new kustomization in the current directory
---

* `kustomize create`
  * -- will create a -- NEW `kustomization.yaml` (empty) | current directory
  * `kustomize create --namespace=myapp --resources=deployment.yaml,service.yaml --labels=app:myapp` 
    * _Example:_ 
      ```
      apiVersion: kustomize.config.k8s.io/v1beta1
      kind: Kustomization
      resources:
      - deployment.yaml
      - service.yaml
      namespace: myapp
      commonLabels:
        app: myapp
      ```

* if you want to update it MANUALLY -> `kustomize edit` sub-commands
