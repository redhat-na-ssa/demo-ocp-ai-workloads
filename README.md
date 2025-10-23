# Demo - AI Workloads on OpenShift

This repo is intented to demonstrate the capibilities for OpenShift for running AI workloads
based on the docs [OpenShift - Ai Workloads](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/ai_workloads/index)

## Prerequisites - Get a cluster

- OpenShift 4.14+
  - role: `cluster-admin` - for all [demo](demos) or [cluster](clusters) configs
  - role: `self-provisioner` - for namespaced components

[Red Hat Demo Platform](https://demo.redhat.com) Options (Tested)

NOTE: The node sizes below are the **recommended minimum** to select for provisioning

- <a href="https://demo.redhat.com/catalog?item=babylon-catalog-prod/sandboxes-gpte.sandbox-ocp.prod&utm_source=webapp&utm_medium=share-link" target="_blank">AWS with OpenShift Open Environment</a>
  - 1 x Control Plane - `m6a.2xlarge`
  - 0 x Workers - `m6a.2xlarge`
- <a href="https://demo.redhat.com/catalog?item=babylon-catalog-prod/sandboxes-gpte.ocp4-single-node.prod&utm_source=webapp&utm_medium=share-link" target="_blank">One Node OpenShift</a>
  - 1 x Control Plane - `m6a.2xlarge`
- <a href="https://catalog.demo.redhat.com/catalog?item=babylon-catalog-prod/openshift-cnv.ocp4-cnv-gitops.prod&utm_source=webapp&utm_medium=share-link" target="_blank">OpenShift GitOps Blank Environment</a>
  - 1 x Control Plane - `16 cores`, `64Gi`

## Getting Started

### Install the [OpenShift Web Terminal](https://docs.openshift.com/container-platform/4.12/web_console/web_terminal/installing-web-terminal.html)

The following icon should appear in the top right of the OpenShift web console after you have installed the operator. Clicking this icon launches the web terminal.

![Web Terminal](docs/images/web-terminal.png "Web Terminal")

NOTE: Reload the page in your browser if you do not see the icon after installing the operator.

## Make the enhanced web terminal permanent

> [!IMPORTANT]  
> Run the following commands from the enhanced web terminal

```sh
# apply the enhanced web terminal
until oc apply -k https://github.com/redhat-na-ssa/demo-ocp-ai-workloads/demo/web-terminal; do : ; done

# reset the web terminal (from the web terminal)
$(wtoctl | grep 'oc delete')
```

Setup at least 1 worker and isolate the control plane

```sh
ocp_machineset_scale 1
ocp_control_nodes_not_schedulable
oc apply -k ../demo_ops/components/cluster-configs/autoscale/overlays/default
```

Setup Nvidia GPU Node

```sh
# setup gpu node
ocp_aws_machineset_create_gpu
ocp_machineset_scale 1
```

## Demo Quickstart

```sh
until oc apply -k demo; do : ; done
```

## Additional Info

See [NOTES](docs/NOTES.md)
See [KUEUE](docs/KUEUE.md)
