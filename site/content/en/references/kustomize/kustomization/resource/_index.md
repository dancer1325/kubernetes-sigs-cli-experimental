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

* read & processed | depth-first order

[hashicorp/go-getter]: https://github.com/hashicorp/go-getter#url-format
[remoteBuild.md]: https://github.com/kubernetes-sigs/kustomize/blob/master/examples/remoteBuild.md
