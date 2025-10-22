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
