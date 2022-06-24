<!-- vim: set ft=Markdown ts=2 -->

# Ansible Role - jpclipffel.k8s.microk8s

Deploys and maintains a MicroK8s cluster.

## 5 minutes how-to

Start by reading the [collection documentation](../../README.md)

### MicroK8s addons

You can enable (new) addons during or after the cluster creation using the tags
`setup` and/or `apply`.

Use the variable `jpclipffel_k8s_microk8s_addons` to specify which addons to
install; Example:

```yaml
jpclipffel_k8s_microk8s_addons:
  - "dns"       # Enable the cluster DNS
  - "rbac"      # Enable K8S RBAC
  - "storage"   # Cluster local StorageClass
```

The complete list of addons is at https://microk8s.io/docs/addons

### Containerd configuration

MicroK8s uses _Containerd_, which is installed alongside MicroK8s in its
_Snap_ directory (e.g. `/var/snap/microk8s/current/`).

_Containerd_ can be configured using the role `jpclipffel_k8s_containerd`
with the following variables set at the cluster level:

```yaml
# Do not install Containerd, just configures it
jpclipffel_container_containerd_configonly: true

# Instruct the role where Containerd config is installed
jpclipffel_container_containerd_root_path: "/var/snap/microk8s/current/args"
jpclipffel_container_containerd_config_path: "{{ jpclipffel_container_containerd_root_path }}/containerd-template.toml"

# Configure Containerd
jpclipffel_container_containerd_certs:
"gitlab.tld:5050": |
  server = "https://gitlab.tld:5050"
  [host."http://gitlab.tld:5050"]
    capabilities = ["pull", "resolve"]
jpclipffel_container_containerd_auths: |
  [plugins."io.containerd.grpc.v1.cri".registry.configs."gitlab.tld:5050".auth]
    username = "foo"
    password = "bar"
  # [plugins."io.containerd.grpc.v1.cri".registry.configs."gitlab.tld:5050".tls]
  #   insecure_skip_verify = true
  # ...
```

### Setup a new cluster

* Create an inventory as shown in the [collection documentation](../../README.md)
* Add the variables describe in the rpevious sections to configure MicroK8s
* Create a playbook:
  ```yaml
  - hosts: microk8s
    roles:
      - jpclipffel.k8s.microk8s
  ```
* Run the playbook:
  ```shell
  $> ansible-playbook -i <inventory> <playbook.yml> --tags 'setup,apply'
  ```

## Variables

The complete list of variables is defined in the role [`defaults/mains.yml`](./defaults/main.yml)

## Tags

| Tags       | Description                             |
| ---------- | --------------------------------------- |
| `setup`    | Deploy and configure a MicroK8s cluster |
| `apply`    | Enable MicroK8s addons                  |
| `restart`  | Restart MicroK8s                        |
| `stats`    | Retrieve and set cluster info           |
| `facts`    | Retrieve and set cluster info           |
| `delete`   | Disable MicroK8s addons                 |
| `teardown` | Stop a MicroK8s cluster                 |
| `remove`   | Stop and remove a MicroK8s cluster      |
