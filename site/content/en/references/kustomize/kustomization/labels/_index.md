---
title: "labels"
linkTitle: "labels"
type: docs
weight: 10
description: >
    Add labels and optionally selectors to all resources.
---

* `labels`
  * allows
    * adding labels WITHOUT AUTOMATICALLY injecting CORRESPONDING selectors 
      * 👀if ALL flags `false` -> add label | `metadata.labels`👀 
  * 👀ALLOWED flags 👀
    * `includeTemplates`
      * if `true` -> labels added ALSO | `metadata.labels` & `spec.template.metadata.labels`
      * by default, `false`
    * `includeSelectors`: 
      * if `true` -> labels added ALSO | `metadata.labels`, `spec.template.metadata.labels` & selectors
        * if you change to `true` | live resources -> it could throw an error
          * Reason: 🧠NOT recommended to change selectors | live resources 🧠
      * by default, `false`

* `commonLabels`
  * ALTERNATIVE to `labels`
  * ALWAYS adds selectors 
  * ⚠️NOT recommended⚠️
    * Reason: 🧠if resource has been applied | cluster -> selectors for resources (_Example:_ deployments & Services) should NOT be changed 🧠

* _Examples:_
  * Example1:_ [selectors and templates NOT modified](selectorAndTemplatesNotModified)
  * _Example2:_ [selectors modified](selectorsModified)
  * _Example3:_ [templates modified](templatesModified)
