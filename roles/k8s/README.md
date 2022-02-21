# Ansible role - `k8s_base`

Ansible role to deploy, scale and manage a Kubernetes cluster.

## Usage

This role may be executed on an existing Kubernetes cluster, or on unconfigured hosts.

It distinguishes between *masters* and *workers* nodes using the host variable `jpclipffel_k8s_k8s_node_type` (which should be either `master` or `worker`).


## Tags

| Tag        | Description                      |
|------------|----------------------------------|
| `stats`    | Collect and set custom stats     |
| `setup`    | Install and configure Kubernetes |
| `teardown` | Deletes the cluster              |
| `remove`   | Removes all traces of Kubernetes |

## Variables

| Variable                          | Type     | Default              | Required | Description                          |
|-----------------------------------|----------|----------------------|----------|--------------------------------------|
| `jpclipffel_k8s_k8s_node_type`              | `string` |                      | Yes      | K8S node type (`master` or `worker`) |
| `jpclipffel_k8s_k8s_control_plane_endpoint` | `string` |                      | Yes      | Control plane (IP or FQDN)           |
| `jpclipffel_k8s_k8s_kubeconfig`             | `string` | `$HOME/.kube/config` | No       | *kubeconfig* file                    |

## Sequence

```text
main
|
| ------------------------------ tag: teardown --
\__ teardown
| -----------------------------------------------
|
|
| -------------------------------- tag: remove --
\__ remove
| -----------------------------------------------
|
|
| --------------------------------- tag: setup --
\__ assert
|
\__ facts
|
\__ install
|
\__ cluster
  -----------------------------------------------
```
