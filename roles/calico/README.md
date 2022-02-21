# Ansible Role - jpclipffel.k8s.calico

Install Calico on Kubernetes clusters.

## Usage

Include the role in your playbook using the `include_role` module (do not specify the `file` attribute):

```yaml
include_role:
  name: k8s_calico
```

Do not forget to set the common `k8s_` variables:

* `k8s_primary_master_name`
* `k8s_node_type`

## Tags

|Tag      |Description|
|---------|-----------|
|`apply`  |Install Calico|
|`delete` |Uninstall Calico|

## Variables

|Variable|Type|Required|Defaut|Description|
|--------|----|--------|------|-----------|
|`k8s_primary_master_name`|`string`|Yes|-|K8S cluster primary master name (as set in Ansible inventory)|
|`k8s_node_type`|`string`|Yes|-|Host role in K8S cluster (`master` or `worker`)|

## Templates

|Template|Description|
|--------|-----------|
|`calico.conf.j2`|Configuration file for `NetworkManager`|
