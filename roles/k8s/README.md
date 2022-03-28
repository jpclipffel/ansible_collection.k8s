# Ansible Role - jpclipffel.k8s.k8s

Ansible role to deploy, scale and manage a Kubernetes cluster.

## 5 minutes how-to

Start by reading the [collection documentation](../../README.md)

> The cluster host(s) must have a compatible CRI installed (_Containerd_, _Podman_, etc.).  
> The collection [`jpclipffel.container`](https://github.com/jpclipffel/ansible_collection.container)
  can be used to install and setup a CRI.

### Setup a new cluster

Create an inventory as shown in the [collection documentation](../../README.md)

Create a playbook:

```yaml
- hosts: k8s
  roles:
    - jpclipffel.container.containerd
    - jpclipffel.k8s.k8s
    - jpclipffel.k8s.calico
```

Run the playbook:

```shell
$> ansible-playbook -i <inventory> <playbook.yml> --tags 'setup,apply'
```

## Inventory setup

This role may be executed on an existing Kubernetes cluster, or on unconfigured hosts.

It distinguishes between *masters* and *workers* nodes using either:
* The host variable `jpclipffel_k8s_k8s_node_type` which should be either `master` or `worker`
* The host groups which should be either `k8s_master` or `k8s_worker`

## Variables

The complete list of variables is defined in the role [`defaults/mains.yml`](./defaults/main.yml)

## Tags

| Tag        | Description                      |
|------------|----------------------------------|
| `stats`    | Collect and set custom stats     |
| `setup`    | Install and configure Kubernetes |
| `teardown` | Deletes the cluster              |
| `remove`   | Removes all traces of Kubernetes |
