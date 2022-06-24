<!-- vim: set ft=Markdown ts=4 -->

# Ansible Collection - jpclipffel.k8s

This collection deploys, maintains and manages K8S clusters.

## 5 minutes how-to

### Requirements

* The cluster host(s) must have a compatible CRI installed (_Containerd_, _Podman_, etc.)
  * The collection [`jpclipffel.container`][jpclipffel.container]
    can be used to install and setup a CRI

### Inventory setup

Create an inventory which include the host(s) to use as K8S cluster node(s):

```yaml
all:
  children:

    # For MicroK8s cluster:
    microk8s:
      vars:
        # Optional: MicroK8s configuration variables
        # jpclipffel_k8s_microk8s_*: ...
      hosts:
        microk8s-01.tld:
        microk8s-02.tld:
        # ...

    # For vanilla (kubeadm) cluster:
    k8s:
      vars:
        # Optional: K8S cluster configuration variables
        # jpclipffel_k8s_k8x_*: ...
      children:
        k8s_masters:
          vars:
            jpclipffel_k8s_k8x_node_type: master
          hosts:
            master-01.tld:
            master-02.tld:
            # ...
        k8s_workers:
          vars:
            jpclipffel_k8s_k8x_node_type: worker
          hosts:
            worker-01.tld:
            worker-02.tld:
            # ...
```

Clusters setup is done using the following roles and collections:

| Resource           | Variables name format       | Documentation / link             |
|--------------------|-----------------------------|----------------------------------|
| `microk8s` cluster | `jpclipffel_k8s_microk8s_*` | [Link](roles/microk8s/README.md) |
| `vanilla` cluster  | `jpclipffel_k8s_k8x_*`      | [Link](roles/k8x/README.md)      |

### Playbook setup

Create an Ansible playbook:

```yaml
# For MicroK8s cluster:
- hosts: microk8s
  vars:
    jpclipffel_container_containerd_configonly: true
    jpclipffel_container_containerd_root_path: "/var/snap/microk8s/current/args"
    jpclipffel_container_containerd_config_path: "{{ jpclipffel_container_containerd_root_path }}/containerd-template.toml"
  roles:
    - jpclipffel.k8s.microk8s           # Setup the single-node cluster
    - jpclipffel.container.containerd   # Setup MicroK8s CRI (Containerd)


# For vanilla (kubeadm) cluster:
- hosts: k8s
  roles:
    - jpclipffel.container.containerd   # Setup the Containerd CRI
    - jpclipffel.k8s.k8x                # Setup the K8S cluster
```

### Deploy

Ensure the required collections are installed:

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

### `k8x`

* Deploys and maintains a vanilla K8S cluster
* See [the role documentation](roles/k8s/README.md)

---

[jpclipffel.container]: https://github.com/jpclipffel/ansible_collection.container
