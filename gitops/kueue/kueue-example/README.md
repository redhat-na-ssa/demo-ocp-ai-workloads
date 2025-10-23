# Introduction

This example helps demonstrate quota allocation using Kueue with preemption.

## Overview

In this example, there are 2 teams that work in their own namespace:

1. Team A and B belongs to the same [cohort](cluster/cohort-ab.yaml) [[docs](https://kueue.sigs.k8s.io/docs/concepts/cluster_queue/#cohort)]
1. Both teams [share a quota](cluster/que-shared.yaml)
1. Team A has [quota to 4 GPU](cluster/que-team-a.yaml) while [team B has no quota](cluster/que-team-b.yaml)
1. Team A can preempt others

### Kueue Configuration

There are 2 `ResourceFlavor` that manages the [CPU/Memory](cluster/flavor-default.yaml) and [GPU](cluster/flavor-gpu.yaml) resources. The GPU `ResourceFlavor` tolerates nodes that have been tainted with `nvidia.com/gpu`.

Both teams have their individual cluster queue that is associated with their respective namespace.

| Name                        | CPU | Memory (GB) | GPU
| --------------------------- | --- | ----------- | ---
| [Que - Team A](cluster/que-team-a.yaml)   | 0   | 0   | 4
| [Que - Team B](cluster/que-team-b.yaml)   | 0   | 0   | 0
| [Que - Shared](cluster/que-shared.yaml)   | 10  | 64  | 0
