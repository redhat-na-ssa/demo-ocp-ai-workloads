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

RHOAI + Kueue

https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/2.23/html/managing_openshift_ai/managing-workloads-with-kueue_kueue

```docs
Unmanaged

In this mode, OpenShift AI integrates with an existing Kueue installation managed by the Red Hat build of Kueue Operator. The Red Hat build of Kueue Operator must be installed and running on the cluster.

When Unmanaged mode is enabled, the OpenShift AI Operator creates a default Kueue custom resource (CR) if one does not already exist. This prompts the Red Hat build of Kueue Operator to install its controller manager and activate Kueue. As a result, a cluster administrator only needs to install the Red Hat build of Kueue Operator and set the managementState to Unmanaged.
```

## Links

- https://ai-on-openshift.io/odh-rhoai/kueue-preemption/readme/
- https://kueue.sigs.k8s.io/docs/concepts/cluster_queue/
- https://github.com/kubernetes-sigs/kueue/issues/2681
