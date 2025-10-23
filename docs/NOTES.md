# Working Notes for building the demo

## RHOAI + Kueue

[RHOAI Docs - Kueue](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/2.23/html/managing_openshift_ai/managing-workloads-with-kueue_kueue)

Consider the following if using Kueue with OpenShift AI integrations.

> [!IMPORTANT]
> OpenShift AI 2.23 and earlier versions include an embedded Kueue component for managing distributed workloads. Starting with OpenShift AI 2.24, the embedded Kueue component will be deprecated and Kueue will be provided through Red Hat build of Kueue, which is installed and managed by the Red Hat build of Kueue Operator.
>
>You cannot install both the embedded Kueue and the Red Hat build of Kueue Operator on the same cluster because this creates conflicting controllers that manage the same resources.

### Links

- [Deploy a machine learning model by using KServe RawDeployment](https://access.redhat.com/solutions/7078183)

### Kueue management modes

`Managed`
> OpenShift AI deploys and manages the embedded Kueue component. This is the current default but is being deprecated.

`Unmanaged`
> OpenShift AI integrates with the Red Hat build of Kueue Operator that you install separately.

`Removed`
> The Kueue component is disabled uninstalled from OpenShift AI.

Example `DataScienceCluster`

```yaml
apiVersion: datasciencecluster.opendatahub.io/v1
kind: DataScienceCluster
metadata:
  name: default-dsc
spec:
  components:
    kueue:
      managementState: Unmanaged
    kserve:
      defaultDeploymentMode: RawDeployment
      managementState: Managed
      serving:
        managementState: Removed
        name: knative-serving
```

Example `OdhDashboardConfig`

```yaml
apiVersion: opendatahub.io/v1alpha
kind: OdhDashboardConfig
metadata:
  name: odh-dashboard-config
spec:
  dashboardConfig:
    disableHardwareProfiles: false
    disableKueue: false
```

Example `DSCInitialization`

```yaml
apiVersion: dscinitialization.opendatahub.io/v1
kind: DSCInitialization
metadata:
  name: default-dsci
spec:
  serviceMesh:
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
