# Working Notes for building the demo

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
- Error logs from the operator w/o cert manager installed
  - `please make sure that cert-manager is installed on your cluster`

## Links

- https://ai-on-openshift.io/odh-rhoai/kueue-preemption/readme/
- https://kueue.sigs.k8s.io/docs/concepts/cluster_queue/
- https://github.com/kubernetes-sigs/kueue/issues/2681
