# Ansible Collection - jpclipffel.k8s

This deploys, maintains and manages K8S clusters.
Various K8S deployemtn are supported.

## Roles

### `microk8s`
Deploys and maintains a MicroK8s cluster (or single node).

| Tags       | Description                        |
|------------|------------------------------------|
| `setup`    | Deploy a MicroK8s cluster          |
| `teardown` | Stop a MicroK8s cluster            |
| `remove`   | Stop and remove a MicroK8s cluster |

### `k8s`
Deploys and maintains a vanilla K8S cluster.

| Tags       | Description                           |
|------------|---------------------------------------|
| `setup`    | Deploy a vanilla K8S cluster          |
| `teardown` | Stop a vanilla K8S cluster            |
| `remove`   | Stop and remove a vanilla K8S cluster |

### `metallb`
Deploys and maintains MetalLB on a K8S cluster.

### `calico`
Deploys and maintains the Calico CNI on a K8S cluster.