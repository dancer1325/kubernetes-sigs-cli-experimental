---
title: "bases"
linkTitle: "bases"
type: docs
weight: 1
description: >
    Add resources from a kustomization dir.
---

* `bases` field
  * | v2.1.0, 
    * ⚠️deprecated ⚠️
  * | Kustomization API
    * `kustomize.config.k8s.io/v1beta1`
      * NOT removed == EXIST FOREVER
    * `kustomize.config.k8s.io/v1`
      * NOT included
  * recommendations
    * `kustomize edit fix`
      * 💡`bases` -- are converted AUTOMATICALLY to -- `resources` 💡
      * Reason: 🧠[base concept](/references/kustomize/glossary#base) exist -- through -- `resources`🧠
