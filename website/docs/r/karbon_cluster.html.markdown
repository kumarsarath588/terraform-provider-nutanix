---
layout: "nutanix"
page_title: "NUTANIX: nutanix_karbon_cluster"
sidebar_current: "docs-nutanix-resource-karbon-cluster"
description: |-
  Provides a Nutanix Karbon Cluster resource to Create a k8s cluster.
---

# nutanix_karbon_cluster

Provides a Nutanix Karbon Cluster resource to Create a k8s cluster.
**Note:** Minimum tested version is Karbon 2.2

## Example Usage

```hcl
data "nutanix_karbon_clusters" "clusters" {}

resource "nutanix_karbon_cluster" "vm1" {
}

```

## Argument Reference

The following arguments are supported:

* `name`: - (Required) The name for the k8s cluster.
* `wait_timeout_minutes`: - (Optional) Maximum wait time for the Karbon cluster to provision.
* `version`: - (Required) K8s version of the cluster.
* `storage_class_config`: - (Required) Storage class configuration attribute for defining the persistent volume attributes.
* `single_master_config`: - (Optional) Configuration of a single master node.
* `active_passive_config`: - (Optional) The active passive mode uses the Virtual Router Redundancy Protocol (VRRP) protocol to provide high availability of the master.
* `external_lb_config`: - (Optional) The external load balancer configuration in the case of a multi-master-external-load-balancer type master deployment.
* `private_registry`: - (Optional) Allows the Karbon cluster to pull images of a list of private registries.
* `etcd_node_pool`: - (Required) Configuration of the node pools that the nodes in the etcd cluster belong to. The etcd nodes require a minimum of 8,192 MiB memory and 409,60 MiB disk space.
* `master_node_pool`: - (Required) Configuration of the master node pools.
* `cni_config`: - (Required) K8s cluster networking configuration. The flannel or the calico configuration needs to be provided.

### Storage Class Config

The storage_class_config attribute supports the following:

* `name`: - (Required) The name of the storage class.
* `reclaim_policy` - (Optional) Reclaim policy for persistent volumes provisioned using the specified storage class.
* `volumes_config.#.file_system` - (Optional) Karbon uses either the ext4 or xfs file-system on the volume disk.
* `volumes_config.#.flash_mode` - (Optional) Pins the persistent volumes to the flash tier in case of a `true` value.
* `volumes_config.#.password` - (Required) The password of the Prism Element user that the API calls use to provision volumes.
* `volumes_config.#.prism_element_cluster_uuid` - (Required) The universally unique identifier (UUID) of the Prism Element cluster.
* `volumes_config.#.storage_container` - (Required) Name of the storage container the storage container uses to provision volumes.
* `volumes_config.#.username` - (Required) Username of the Prism Element user that the API calls use to provision volumes.


### External LB Config

The external load balancer configuration in the case of a multi-master-external-load-balancer type master deployment.

* `external_lb_config.#.external_ipv4_address`: (Required) The external load balancer IPV4 address.
* `external_lb_config.#.master_nodes_config`: (Required) The configuration of the master nodes.
* `external_lb_config.#.master_nodes_config.ipv4_address`: (Required) The IPV4 address to assign to the master.
* `external_lb_config.#.master_nodes_config.node_pool_name`: (Optional) The name of the node pool in which this master IPV4 address will be used.

### private_registry
User inputs of storage configuration parameters for VMs.

* `private_registry`: - (Optional) List of private registries.
* `private_registry.registry_name`: - (Required) Name of the private registry to add to the Karbon cluster.

### Node Pool

The `etcd_node_pool`, `master_node_pool`, `worker_node_pool` attribute supports the following:

* `name`: - (Optional) Unique name of the node pool.
* `node_os_version`: - (Required) The version of the node OS image.
* `num_instances`: - (Required) Number of nodes in the node pool.
* `ahv_config`: - (Optional) VM configuration in AHV.
* `ahv_config.cpu`: - (Required) The number of VCPUs allocated for each VM on the PE cluster.
* `ahv_config.disk_mib`: - (Optional) Size of local storage for each VM on the PE cluster in MiB.
* `ahv_config.memory_mib`: - (Optional) Memory allocated for each VM on the PE cluster in MiB.
* `ahv_config.network_uuid`: - (Required) The UUID of the network for the VMs deployed with this resource configuration.
* `ahv_config.prism_element_cluster_uuid`: - (Required) The unique universal identifier (UUID) of the Prism Element cluster used to deploy VMs for this node pool.
* `nodes`: - List of the deployed nodes in the node pool.
* `nodes.hostname`: - Hostname of the deployed node.
* `nodes.ipv4_address`: - IP of the deployed node.

### cni_config

 The boot_device_disk_address attribute supports the following:

* `node_cidr_mask_size`: - (Optional) The size of the subnet from the pod_ipv4_cidr assigned to each host. A value of 24 would allow up to 255 pods per node.
* `pod_ipv4_cidr`: - (Optional) CIDR for pods in the cluster.
* `service_ipv4_cidr`: - (Optional) Classless inter-domain routing (CIDR) for k8s services in the cluster.
* `flannel_config`: - (Optional) Configuration of the flannel container network interface (CNI) provider.
* `calico_config`: - (Optional) Configuration of the calico CNI provider.
* `calico_config.ip_pool_config`: - (Optional) List of IP pools to be configured/managed by calico.
* `calico_config.ip_pool_config.cidr`: - (Optional) IP range to use for this pool, it should fall within pod cidr.

See detailed information in [Nutanix Karbon Cluster](https://www.nutanix.dev/reference/karbon/api-reference/cluster/).
