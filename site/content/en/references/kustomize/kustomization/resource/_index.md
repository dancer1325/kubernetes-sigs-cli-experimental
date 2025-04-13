---
title: "resources"
linkTitle: "resources"
type: docs
weight: 20
description: >
    Resources to include.
---

* ALLOWED values
  * path to a _file_ / 
    * contain k8s resources | YAML form
      * if you want to add MULTIPLE -> separate -- by -- `---`
  * path (or URL) -- referring to -- another kustomization _directory_
    * ALLOWED URL -- see [remoteBuild.md] --
      * HTTPS git clone URL 
      * SSH git clone URL

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
# Local files
- myNamespace.yaml
- deployment.yaml
- sub-dir/some-deployment.yaml

# Local directories
- ../../commonbase

# Remote URLs
- https://github.com/kubernetes-sigs/kustomize//examples/multibases/?timeout=120&ref=v3.3.1

# Legacy hashicorp/go-getter format
- github.com/kubernets-sigs/kustomize/examples/helloWorld?ref=test-branch
```

* read & processed | depth-first order

[hashicorp/go-getter]: https://github.com/hashicorp/go-getter#url-format
[remoteBuild.md]: https://github.com/kubernetes-sigs/kustomize/blob/master/examples/remoteBuild.md
