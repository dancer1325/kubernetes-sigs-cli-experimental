---
title: "Glossary"
linkTitle: "Glossary"
type: docs
weight: 3
description: >
    Glossary of terms
---

# Glossary

[CRD spec]: https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/
[CRD]: #custom-resource-definition
[DAM]: #declarative-application-management
[Declarative Application Management]: https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/declarative-application-management.md
[JSON]: https://www.json.org/
[JSONPatch]: https://tools.ietf.org/html/rfc6902
[JSONMergePatch]: https://tools.ietf.org/html/rfc7386
[Resource]: #resource
[YAML]: http://www.yaml.org/start.html
[application]: #application
[apply]: #apply
[apt]: https://en.wikipedia.org/wiki/APT_(Debian)
[base]: #base
[bases]: #base
[bespoke]: #bespoke-configuration
[gitops]: #gitops
[k8s]: #kubernetes
[kubernetes]: #kubernetes
[kustomize]: #kustomize
[kustomization]: #kustomization
[kustomizations]: #kustomization
[off-the-shelf]: #off-the-shelf-configuration
[overlay]: #overlay
[overlays]: #overlay
[patch]: #patch
[patches]: #patch
[patchJson6902]: #patchjson6902
[patchExampleJson6902]: https://github.com/kubernetes-sigs/kustomize/tree/master/examples/jsonpatch.md
[patchesJson6902]: #patchjson6902
[proposal]: https://github.com/kubernetes/community/pull/1629
[rebase]: https://git-scm.com/docs/git-rebase
[resource]: #resource
[resources]: #resource
[root]: #kustomization-root
[rpm]: https://en.wikipedia.org/wiki/Rpm_(software)
[strategic-merge]: https://git.k8s.io/community/contributors/devel/sig-api-machinery/strategic-merge-patch.md
[target]: #target
[transformer]: #transformer
[variant]: #variant
[variants]: #variant
[workflow]: /kustomize/guides

## application

An _application_ is a group of k8s resources related by
some common purpose, e.g.  a load balancer in front of a
webserver backed by a database.
[Resource] labelling, naming and metadata schemes have
historically served to group resources together for
collective operations like _list_ and _remove_.

This [proposal] describes a new k8s resource called
_application_ to more formally describe this idea and
provide support for application-level operations and
dashboards.

[kustomize] configures k8s resources, and the proposed
application resource is just another resource.

## apply

* == kubectl command / mutate a cluster
* == k8s' foundation of level-based state management 

## base

* == [kustomization] / referred to by some OTHER [kustomization]
  * _Example:_ kustomization / includes an [overlay]

## bespoke configuration

A _bespoke_ configuration is a [kustomization] and some
[resources] created and maintained internally by some
organization for their own purposes.

The [workflow] associated with a _bespoke_ config is
simpler than the workflow associated with an
[off-the-shelf] config, because there's no notion of
periodically capturing someone else's upgrades to the
[off-the-shelf] config.

## custom resource definition

One can extend the k8s API by making a
Custom Resource Definition ([CRD spec]).

This defines a custom [resource] (CD), an entirely
new resource that can be used alongside _native_
resources like ConfigMaps, Deployments, etc.

Kustomize can customize a CD, but to do so
kustomize must also be given the corresponding CRD
so that it can interpret the structure correctly.

## declarative application management

Kustomize aspires to support [Declarative Application Management],
a set of best practices around managing k8s clusters.

In brief, kustomize should

* Work with any configuration, be it bespoke,
   off-the-shelf, stateless, stateful, etc.
* Support common customizations, and creation of
   [variants] (e.g. _development_ vs.
   _staging_ vs. _production_).
* Expose and teach native k8s APIs, rather than
   hide them.
* Add no friction to version control integration to
   support reviews and audit trails.
* Compose with other tools in a unix sense.
* Eschew crossing the line into templating, domain
   specific languages, etc., frustrating the other
   goals.

## generator

* == generator -- of -- NEW resources to create

## gitops

Devops or CICD workflows that use a git repository as a
single source of truth and take action (e.g., build,
test or deploy) when that truth changes.

## kustomization == `kustomization.yaml` 

* ways to share it
  * `kustomization.yaml`,
  * tarball / contains the YAML + references
  * git archive (ditto)
  * URL -- to a -- git repo

* 's [fields](fields.md)
  * categories
    * [resources](#resource) 
      * _Example:_ resources, crds
    * [generators](#generator)
      * _Example:_ configMapGenerator(legacy), secretGenerator(legacy), generators (v2.1).
    * [transformers](#transformer)
      * _Example fields:_ _namePrefix_, _nameSuffix_, _images_,
         _commonLabels_, _patchesJson6902_, etc., _transformers_ (v2.1)
    * meta
      * == fields / -- may influence -- ALL or SOME PREVIOUS ones
      * _Example:_ _vars_, _namespace_, _apiVersion_, _kind_, etc.

## kustomization root

* := directory / IMMEDIATELY contains a `kustomization.yaml` + ALL relative files

* data files
  * must be | kustomization root
  * _Example of data files:_ 
    * resource YAML files,
    * text files / contain _name=value_ pairs -- for a -- ConfigMap or Secret, 
    * files / used | patch transformation
  * -- are specified via -- relative paths

* `--load_restrictions none`
  * == special flag /
    * relax this security feature
    * enable a patch file -- to be shared by -- >1 kustomization

* ways to refer OTHER kustomizations
  * absolute path
  * relative path

* if kustomization __A__ -- depends on -- kustomization __B__ ->
  * ❌ __B__ may NOT _contain_ __A__ ❌
  * ❌__B__ may NOT _depend on_ __A__ ❌

* if __A__ contains __B__ -> recommended to
  * __A__ -- directly depends on -- __B__'s resources
  * eliminate __B__'s "kustomization.yaml"
    * == absorb __B__ | __A__

* COMMON layout
    > ```
    > ├── base
    > │   ├── deployment.yaml
    > │   ├── kustomization.yaml
    > │   └── service.yaml
    > └── overlays
    >     ├── dev
    >     │   ├── kustomization.yaml
    >     │   └── patch.yaml
    >     ├── prod
    >     │   ├── kustomization.yaml
    >     │   └── patch.yaml
    >     └── staging
    >         ├── kustomization.yaml
    >         └── patch.yaml
    > ```

  * the roots `dev`, `prod` & `staging` -- refer to the -- `base` root

## kubernetes

[Kubernetes](https://kubernetes.io) is an open-source
system for automating deployment, scaling, and
management of containerized applications.

It's often abbreviated as _k8s_.

## kubernetes-style object

[fields required]: https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/#required-fields

An object, expressed in a YAML or JSON file, with the
[fields required] by kubernetes.  Basically just a
_kind_ field to identify the type, a _metadata/name_
field to identify the particular instance, and an
_apiVersion_ field to identify the version (if there's
more than one version).

## kustomize

* == CL tool / 
  * support
    * template-free,
    * declarative configuration / 
      * structured customization 
      * -- target to -- k8s-style objects
        * == understand API resources

* == [DAM]'s implementation 

## off-the-shelf configuration

An _off-the-shelf_ configuration is a kustomization and
resources intentionally published somewhere for others
to use.

E.g. one might create a github repository like this:

> ```
> github.com/username/someapp/
>   kustomization.yaml
>   deployment.yaml
>   configmap.yaml
>   README.md
> ```

Someone could then _fork_ this repo (on github) and
_clone_ their fork to their local disk for
customization.

This clone could act as a [base] for the user's
own [overlays] to do further customization.

## overlay

* := kustomization / -- depends on -- ANOTHER kustomization
* requirements
  * ⚠️its bases ⚠️
    * OTHERWISE, it's unusable 
* use cases
  * \>1 overlays
    * Reason: 🧠create base's DIFFERENT [variants] 🧠
    * _Example:_ _development_, _QA_, _staging_ and
_production_ environment variants
* _Example:_ configures a cluster
    ```
    kustomize build someapp/overlays/staging |\
      kubectl apply -f -
    kustomize build someapp/overlays/production |\
      kubectl apply -f -
    ```
  * implicitly use the base

## package

The word _package_ has no meaning in kustomize, as
kustomize is not to be confused with a package
management tool in the tradition of, say, [apt] or
[rpm].

## patch

General instructions to modify a resource.

There are two alternative techniques with similar
power but different notation - the
[strategic merge patch](#patchstrategicmerge)
and the [JSON patch](#patchjson6902).

## patchStrategicMerge

A _patchStrategicMerge_ is [strategic-merge]-style patch (SMP).

An SMP looks like an incomplete YAML specification of
a k8s resource.  The SMP includes `TypeMeta`
fields to establish the group/version/kind/name of the
[resource] to patch, then just enough remaining fields
to step into a nested structure to specify a new field
value, e.g. an image tag.

By default, an SMP _replaces_ values.  This is
usually desired when the target value is a simple
string, but may not be desired when the target
value is a list.

To change this
default behavior, add a _directive_.  Recognized
directives in YAML patches are _replace_ (the default)
and _delete_ (see [these notes][strategic-merge]).

Note that for custom resources, SMPs are treated as
[json merge patches][JSONMergePatch].

Fun fact - any resource file can be used as
an SMP, overwriting matching fields in another
resource with the same group/version/kind/name,
but leaving all other fields as they were.

TODO(monopole): add ptr to example.

## patchJson6902

A _patchJson6902_ refers to a kubernetes [resource] and
a [JSONPatch] specifying how to change the resource.

A _patchJson6902_ can do almost everything a
_patchStrategicMerge_ can do, but with a briefer
syntax.  See this [example][patchExampleJson6902].

## plugin

A chunk of code used by kustomize, but not necessarily
compiled into kustomize, to generate and/or transform a
kubernetes resource as part of a kustomization.

Details [here](../../../guides/extending_kustomize).

## resource

* | REST-ful API,
  * == HTTP operation's target object

* | kustomization,
  * == [root] relative path -- to a -- [YAML] OR [JSON] / describe a k8s API object

## root

* [kustomization root][root]

## sub-target / sub-application / sub-package

A _sub-whatever_ is not a thing. There are only
[bases] and [overlays].

## target

* == `kustomize build`'s argument
    > ```
    >  kustomize build $target
    > ```
* ALLOWED
  * values
    * [base] 
    * [overlay]
  * types 
    * path -- to a -- [kustomization]
    * url -- to a -- [kustomization]

## transformer

* allows
  * modifying a resource
  * visiting a resource
  * collecting information about the resource | `kustomize build`

## variant

* := | cluster, (apply [overlay] | [base])'s outcome
* _Example:_ _staging_ & _production_ overlay apply | COMMON base -> create DISTINCT variants
  * _staging_ variant use case
    * quality assurance testing
  * _production_ variant use case
    * production traffic
