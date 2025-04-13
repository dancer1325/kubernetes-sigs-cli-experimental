---
title: "crds"
linkTitle: "crds"
type: docs
weight: 7
description: >
    Adding CRD support
---

* EACH entry | this list    
  * == relative path to a file -- for -- CRD
  * allow
    * kustomize -- be aware of -- CRDs &
    * apply proper transformation | those types' ANY objects

* use case
  * CRD object / -- refers to a -- ConfigMap object
    * 's name may change -- by --
      * adding 
        * namePrefix,
        * nameSuffix
        * hashing

* annotations / can be put | openAPI definitions are
  - "x-kubernetes-annotation": ""
  - "x-kubernetes-label-selector": ""
  - "x-kubernetes-identity": ""
  - "x-kubernetes-object-ref-api-version": "v1",
  - "x-kubernetes-object-ref-kind": "Secret",
  - "x-kubernetes-object-ref-name-key": "name",

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

crds:
- crds/typeA.yaml
- crds/typeB.yaml
```
