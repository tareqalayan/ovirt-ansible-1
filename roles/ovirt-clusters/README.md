oVirt Clusters
==============

The `ovirt-clusters` role is used set up oVirt clusters.

Requirements
------------

 * oVirt Python SDK version 4
 * Ansible version 2.3

Role Variables
--------------

| Name                  | Default value         |  Description                            |
|-----------------------|-----------------------|-----------------------------------------|
| clusters              | UNDEF                 | List of dictionaries that describe the cluster. |
| data_center_name      | UNDEF (Required)      | Name of the data center.                 |
| compatibility_version | UNDEF (Required)      | Compatibility version of data center.    |

The items in `clusters` list can contain the following parameters:

| Name                              | Default value       | Description                             |
|-----------------------------------|---------------------|-----------------------------------------|
| name                              | UNDEF (Required)    | Name of the cluster.                     |
| state                             | present             | State of the cluster.                    |
| cpu_type                          | Intel Conroe Family | CPU type of the cluster.                 |
| profile                           | UNDEF               | The cluster profile. You can choose a predefined cluster profile, see the tables below. |
| ballooning                        | UNDEF               | If True enable memory balloon optimization. Memory balloon is used to re-distribute / reclaim the host memory based on VM needs in a dynamic way. |
| description                       | UNDEF               | Description of the cluster. |
| ksm                               | UNDEF               | I True MoM enables to run Kernel Same-page Merging KSM when necessary and when it can yield a memory saving benefit that outweighs its CPU cost. |
| ksm_numa                          | UNDEF               | If True enables KSM ksm for best berformance inside NUMA nodes. |
| vm_reason                         | UNDEF               | If True enable an optional reason field when a virtual machine is shut down from the Manager, allowing the administrator to provide an explanation for the maintenance. |
| host_reason                       | UNDEF               | If True enable an optional reason field when a host is placed into maintenance mode from the Manager, allowing the administrator to provide an explanation for the maintenance. |
| memory_policy                     | UNDEF               | <ul><li>disabled - Disables memory page sharing.</li><li>server - Sets the memory page sharing threshold to 150% of the system memory on each host.</li><li>desktop - Sets the memory page sharing threshold to 200% of the system memory on each host.</li></ul> |
| migration_policy                  | UNDEF               | A migration policy defines the conditions for live migrating virtual machines in the event of host failure. Following policies are supported:<ul><li>legacy - Legacy behavior of 3.6 version.</li><li>minimal_downtime - Virtual machines should not experience any significant downtime.</li><li>suspend_workload - Virtual machines may experience a more significant downtime.</li><li>post_copy - Virtual machines should not experience any significant downtime. If the VM migration is not converging for a long time, the migration will be switched to post-copy</li></ul> |
| scheduling_policy                 | UNDEF               | The scheduling policy used by the cluster. |
| ha_reservation                    | UNDEF               | If True enable the oVirt/RHV to monitor cluster capacity for highly available virtual machines. |
| fence_enabled                     | UNDEF               | If True, enables fencing on the cluster. |
| fence_skip_if_connectivity_broken | UNDEF               | If True, fencing will be temporarily disabled if the percentage of hosts in the cluster that are experiencing connectivity issues is greater than or equal to the defined threshold. |
| fence_skip_if_sd_active           | UNDEF               | If True, any hosts in the cluster that are Non Responsive and still connected to storage will not be fenced. |
| mac_pool                          | UNDEF               | Mac pool name. |


More information about the parameters can be found in the [Ansible documentation](http://docs.ansible.com/ansible/ovirt_cluster_module.html).

Possible `profile` options are `development` and `production`, their default values are described below:

`Development`:

| Parameter        | Value         |
|------------------|---------------|
| ballooning       | true          |
| ksm              | true          |
| host_reason      | false         |
| vm_reason        | false         |
| memory_policy    | server        |
| migration_policy | post_copy     |

`Production`:

| Parameter                         | Value              |
|-----------------------------------|--------------------|
| ballooning                        | false              |
| ksm                               | false              |
| host_reason                       | true               |
| vm_reason                         | true               |
| memory_policy                     | disabled           |
| migration_policy                  | suspend_workload   |
| scheduling_policy                 | evenly_distributed |
| ha_reservation                    | true               |
| fence_enabled                     | true               |
| fence_skip_if_connectivity_broken | true               |
| fence_skip_if_sd_active           | true               |

Dependencies
------------

No.

Example Playbook
----------------

```yaml
- name: oVirt infra
  hosts: localhost
  connection: local
  gather_facts: false

  vars:
   data_center_name: mydatacenter
   compatibility_version: 4.1

   clusters:
     - name: production
       cpu_type: Intel Conroe Family
       profile: production
       mac_pool: production_mac_pools

  roles:
    - ovirt-clusters
```

License
-------

Apache License 2.0
