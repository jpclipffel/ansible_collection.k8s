# Ansible Collection - jpclipffel.k8s

This collection deploys, maintains and manages K8S clusters.

## 5 minutes how-to

### Requirements

The cluster host(s) must have a compatible CRI installed (_Containerd_, _Podman_, etc.).
The collection [`jpclipffel.container`][jpclipffel.container]
can be used to install and setup a CRI.

### Inventory setup

Create an inventory which include the host(s) to use as K8S cluster node(s):

```yaml
all:
  children:

    # For MicroK8s single-node cluster:
    microk8s:
      hosts:
        microk8s.tld:
          # Optional: MicroK8s configuration variables
          # jpclipffel_k8s_microk8s_*: ...

    # For vanilla (kubeadm) cluster:
    k8s:
      vars:
        # Optional: K8S cluster configuration variables
        # jpclipffel_k8s_k8s_*: ...
      children:
        k8s_masters:
          hosts:
            master-01.tld:
            master-02.tld:
            master-03.tld:
        k8s_workers:
          hosts:
            worker-01.tld:
            worker-02.tld:
            worker-03.tld:
```

Clusters setup is done using the following roles and collections:

| Resource           | Variables name format       | Documentation / link             |
|--------------------|-----------------------------|----------------------------------|
| `microk8s` cluster | `jpclipffel_k8s_microk8s_*` | [Link](roles/microk8s/README.md) |
| `vanilla` cluster  | `jpclipffel_k8s_k8s_*`      | [Link](roles/k8s/README.md)      |
| CRI                | `jpclipffel_container_*`    | [Link][jpclipffel.container]     |

### Playbook setup

Create an Ansible playbook:

```yaml
# For MicroK8s single-node cluster:
- hosts: microk8s
  roles:
    - jpclipffel.k8s.microk8s           # Setup the single-node cluster

# For vanilla (kubeadm) cluster:
- hosts: k8s
  roles:
    - jpclipffel.container.containerd   # Setup the Containerd CRI
    - jpclipffel.k8s.k8s                # Setup the K8S cluster
    - jpclipffel.k8s.calico             # Setup the cluster CNI
```

### Deploy

Ensure the collections are installed:

```shell
$> ansible-galaxy collection install jpclipffel.k8s
$> ansible-galaxy collection install jpclipffel.container
```

Run the playbook with the appropriate tag(s) (see next section for available tags):

```shell
ansible-playbook -i <your-inventory> <your-playbook.yml> --tags 'tag,tag,...'
```

## Roles

### `microk8s`

* Deploys and maintains a MicroK8s node
* See [the role documentation](roles/microk8s/README.md)

### `k8s`

* Deploys and maintains a vanilla K8S cluster
* See [the role documentation](roles/k8s/README.md)

### `metallb`

* Deploys and maintains MetalLB on a K8S cluster

### `calico`

* Deploys and maintains the Calico CNI on a K8S cluster

---

[jpclipffel.container]: https://github.com/jpclipffel/ansible_collection.container