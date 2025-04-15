---
title: "patches"
linkTitle: "patches"
type: docs
weight: 15
description: >
    Patch resources
---

[strategic merge]: /references/kustomize/glossary#patchstrategicmerge
[JSON6902]: /references/kustomize/glossary#patchjson6902

* `patches`
  * == patches or overlays 
  * allows
    * add or override fields | resources
  * == list of patches / 
    * applied | specified order
    * ALLOWED patch
      - [strategic merge] patch OR [JSON6902] patch
      - file OR inline string
      - -- target -- >= 1 resources 
        - -- by --
          - `group`,
          - `version`,
          - `kind`,
          - `name`,
            - -- use -- regular expressions
              - _Example:_ `myapp` == `^myapp$`
          - `namespace`,
            - -- use -- regular expressions
            - _Example:_ `myapp` == `^myapp$`
          - `labelSelector`
          - `annotationSelector`
        - ALL fields -- need to be -- matched

## Name and kind changes

* TODO:
With `patches` it is possible to override the kind or name of the resource it is
editing with the options `allowNameChange` and `allowKindChange`. For example:
```yaml

patches:

```
By default, these fields are false and the patch will leave the kind and name of the resource untouched.

## Name references

A patch can refer to a resource by any of its previous names or kinds.
For example, if a resource has gone through name-prefix transformations, it can refer to the
resource by its current name, original name, or any intermediate name that it had.

## Patching custom resources

[Strategic merge] patches may require additional configuration via [openapi](../openapi) field to work as expected with custom resources. For example, if a resource uses a merge key other than `name` or needs a list to be merged rather than replaced, Kustomize needs openapi information informing it about this.

[JSON6902] patch usage is the same for built-in and custom resources.

## Examples


### Intents

- Make the container image point to a specific version and not to the latest container in the registry. 
- Adding a standard label containing the deployed version.

There are multiple possible strategies that all achieve the same results. 

### Patch using Inline Strategic Merge

```yaml
# kustomization.yaml
resources:
- deployment.yaml
patches:
  - patch: |-
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: dummy-app
        labels:
          app.kubernetes.io/version: 1.21.0
  - patch: |-
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: not-used
      spec:
        template:
          spec:
            containers:
              - name: nginx
                image: nginx:1.21.0
    target:
      labelSelector: "app.kubernetes.io/name=nginx"
```

If a `target` is specified, the `name` contained in the metadata is required but not used.

### Patch using Inline JSON6902

```yaml
# kustomization.yaml
resources:
- deployment.yaml
patches:
  - patch: |-
      - op: add
        path: /metadata/labels/app.kubernetes.io~1version
        value: 1.21.0
    target:
      group: apps
      version: v1
      kind: Deployment
  - patch: |-
      - op: replace
        path: /spec/template/spec/containers/0/image
        value: nginx:1.21.0
    target:
      labelSelector: "app.kubernetes.io/name=nginx"
```

The `target` field is always required for JSON6902 patches.  
A special replacement character `~1` is used to replace `/` in label name.

### Patch using Path Strategic Merge

```yaml
# kustomization.yaml
resources:
- deployment.yaml
patches:
  - path: add-label.patch.yaml
  - path: fix-version.patch.yaml
    target:
      labelSelector: "app.kubernetes.io/name=nginx"
```

As with the Inline Strategic Merge, the `target` field can be omitted.
In that case, the target resource is matched using 
the `apiVersion`, `kind` and `name` from the patch.

```yaml
# add-label.patch.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dummy-app
  labels:
    app.kubernetes.io/version: 1.21.0
```

```yaml
# fix-version.patch.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: not-used
spec:
  template:
    spec:
      containers:
        - name: nginx
          image: nginx:1.21.0
```

As with the Inline Strategic Merge, the `name` field in the patch is not used when a `target` is specified.

### Patch using Path JSON6902

```yaml
# kustomization.yaml
resources:
- deployment.yaml
patches:
  - path: add-label.patch.json
    target:
      group: apps
      version: v1
      kind: Deployment
  - path: fix-version.patch.yaml
    target:
      labelSelector: "app.kubernetes.io/name=nginx"
```

As with Inline JSON6902, the `target` field is mandatory.

```yaml
# add-label.patch.json
[
  {"op": "add", "path": "/metadata/labels/app.kubernetes.io~1version", "value": "1.21.0"}
]
```

```yaml
# fix-version.patch.yaml
- op: replace
  path: /spec/template/spec/containers/0/image
  value: nginx:1.21.0
```

External patch file can be written both as YAML or JSON.
The content must follow the JSON6902 standard.

### Build Output

All four patches strategies lead to the exact same output:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app.kubernetes.io/name: nginx
    app.kubernetes.io/version: 1.21.0
  name: dummy-app
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: nginx
  template:
    metadata:
      labels:
        app.kubernetes.io/name: nginx
    spec:
      containers:
        - image: nginx:1.21.0
          name: nginx
          ports:
            - containerPort: 80
              name: http
```
