# Working Notes for building the demo

## Quickstart

Web Terminal Setup

```sh
until oc apply -k https://github.com/redhat-na-ssa/demo-ocp-ai-workloads/demo/web-terminal; do : ; done
$(wtoctl | grep delete)
```

```sh
until oc apply -k demo; do : ; done
```

## TL;DR

Label a namespace you want kueue to manage

```sh
oc label namespace <namespace> kueue.openshift.io/managed=true
```

## Issues Found

- `namespace` is not valid for a `kueue` CR
  - See [docs](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/ai_workloads/red-hat-build-of-kueue#create-kueue-cr_install-disconnected)
  - See [local example](../gitops/operators/kueue-operator/instance/base/kueue.yaml)
- The kueue operator does not appear to create the cluster roles from the [docs](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/ai_workloads/red-hat-build-of-kueue#authentication-clusterroles)
  - `kueue-batch-user-role`
  - `kueue-batch-admin-role`
