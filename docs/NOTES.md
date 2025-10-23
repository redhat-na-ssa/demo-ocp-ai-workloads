# Working Notes for building the demo

## RHOAI + Kueue

[RHOAI Docs - Kueue](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/2.23/html/managing_openshift_ai/managing-workloads-with-kueue_kueue)

> [!IMPORTANT]
> OpenShift AI 2.23 and earlier versions include an embedded Kueue component for managing distributed workloads. Starting with OpenShift AI 2.24, the embedded Kueue component will be deprecated and Kueue will be provided through Red Hat build of Kueue, which is installed and managed by the Red Hat build of Kueue Operator.
>
>You cannot install both the embedded Kueue and the Red Hat build of Kueue Operator on the same cluster because this creates conflicting controllers that manage the same resources.

### Kueue management modes

> Removed
>
>This mode disables Kueue in OpenShift AI. If the mode was previously Managed, OpenShift AI uninstalls the embedded distribution. If the mode was previously Unmanaged, OpenShift AI stops checking for the external Kueue integration but does not uninstall the Red Hat build of Kueue Operator. An empty managementState value also functions as Removed.

```yaml
apiVersion: datasciencecluster.opendatahub.io/v1
kind: DataScienceCluster
metadata:
  name: default-dsc
spec:
  components:
    kueue:
      managementState: Removed
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

## Links

- https://ai-on-openshift.io/odh-rhoai/kueue-preemption/readme/
- https://kueue.sigs.k8s.io/docs/concepts/cluster_queue/
- https://github.com/kubernetes-sigs/kueue/issues/2681
