---
title: "nameSuffix"
linkTitle: "nameSuffix"
type: docs
weight: 13
description: >
    Appends the value to the names of all resources and references.
---

* `nameSuffix`
  * add suffix names | defined yaml files
    * if the resource type is `ConfigMap` or `Secret` -> suffix is appended BEFORE the content hash 

* _Example:_
  * `kustomize build .`
