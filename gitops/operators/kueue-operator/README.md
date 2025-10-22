# Kueue Operator

Install Kueue Operator.

Do not use the `base` directory directly, as you will need to patch the `channel` based on the version of OpenShift you are using, or the version of the operator you want to use.

The current *overlays* available are for the following channels:

* [stable-v0.1](operator/overlays/stable-v0.1)
* [stable-v0.2](operator/overlays/stable-v0.2)
* [stable-v1.0](operator/overlays/stable-v1.0)
* [stable-v1.1](operator/overlays/stable-v1.1)

## Usage

If you have cloned the `gitops-catalog` repository, you can install Kueue Operator based on the overlay of your choice by running from the root (`gitops-catalog`) directory.

```sh
oc apply -k kueue-operator/operator/overlays/<channel>
```

Or, without cloning:

```sh
oc apply -k https://github.com/redhat-cop/gitops-catalog/kueue-operator/operator/overlays/<channel>
```

As part of a different overlay in your own GitOps repo:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - https://github.com/redhat-cop/gitops-catalog/kueue-operator/operator/overlays/<channel>?ref=main
```
