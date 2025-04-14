---
title: "images"
linkTitle: "images"
type: docs
weight: 9
description: >
    Modify the name, tags and/or digest for images.
---

* images / 
  * NOT create patches
  * allows
    * modify the 
      * name,
      * tags and/or digest for images  
    * set container's image tags

| Field     | Description                                                              | Example Field | Example Result |
|-----------|--------------------------------------------------------------------------|----------| --- |
| `name`    | images -- are matched by -- image name| `name: nginx`| |
| `newTag`  | override the image **tag** or **digest** for images / image name -- matches -- `name`    | `newTag: new` | `nginx:old` -> `nginx:new` |
| `newName` | override the image **name** for images / image name -- matches -- `name`   | `newName: nginx-special` | `nginx:old` -> `nginx-special:old` |

## Setting a Name -- `newName` --

* steps to specify the image's name
  * specify 
    * `newName`
    * old container image's name

## Setting a Tag -- `newTag` -- 

* steps to specify the image's name
  * specify
    * `newTag`
    * old container image's name

## Setting a Digest -- `digest` -- 

* steps to specify the image's name
  * specify
    * `digest`
    * old container image's name

## Setting a Tag -- from the -- latest commit SHA

* TODO:
A common CI/CD pattern is to tag container images with the git commit SHA of source code.  e.g. if
the image name is `foo` and an image was built for the source code at commit `1bb359ccce344ca5d263cd257958ea035c978fd3`
then the container image would be `foo:1bb359ccce344ca5d263cd257958ea035c978fd3`.

A simple way to push an image that was just built without manually updating the image tags is to
download the [kustomize standalone](https://github.com/kubernetes-sigs/kustomize/) tool and run
`kustomize edit set image` command to update the tags for you.

**Example:** Set the latest git commit SHA as the image tag for `foo` images.

```bash
kustomize edit set image foo:$(git log -n 1 --pretty=format:"%H")
kubectl apply -f .
```

## Setting a Tag -- from an -- Environment Variable

It is also possible to set a Tag from an environment variable using the same technique for setting from a commit SHA.

**Example:** Set the tag for the `foo` image to the value in the environment variable `FOO_IMAGE_TAG`.

```bash
kustomize edit set image foo:$FOO_IMAGE_TAG
kubectl apply -f .
```

{{< alert color="success" title="Committing Image Tag Updates" >}}
The `kustomization.yaml` changes *may* be committed back to git so that they
can be audited.  When committing the image tag updates that have already
been pushed by a CI/CD system, be careful not to trigger new builds +
deployments for these changes.
{{< /alert >}}