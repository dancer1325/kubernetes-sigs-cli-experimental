---
title: "helmCharts"
linkTitle: "helmCharts"
type: docs
weight: 8
description: >
    Helm chart inflation generator.
---

[kustomize builtins]: https://kubectl.docs.kubernetes.io/references/kustomize/builtins/#_helmchartinflationgenerator_
[Helm support long term plan]: https://github.com/kubernetes-sigs/kustomize/issues/4401

## Helm Chart Inflation Generator

* == helm features' subset /
  * limited 
  * help -- getting started with -- kustomize
* -- through the -- `helmCharts`
  * [kustomize builtins]
* ⚠️limited support⚠️9  
* steps
    ```sh
    kustomize build --enable-helm
    ```

## Long term support
 
* ❌NOT support -- the -- entire helm feature set ❌

### current builtin
* supported changes
  - bug fixes
  - critical security issues
  - additional fields / 
    - analogous to flags
    - -- passed to -- `helm template`
    - allow arbitrary commands to be executed
* NOT support
  - private repository or registry authentication
  - OCI registries
  - other large features / 
    - increase the complexity of the feature and/or
    - have significant security implications

### Future support
* helm inflation generator -- will be refactored to a -- KRM function 
* [Helm support long term plan]
