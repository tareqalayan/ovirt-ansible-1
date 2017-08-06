oVirt Mac Pools
=================

The `ovirt-mac-pools` role is used to set up oVirt mac pools.

Requirements
------------

 * oVirt Python SDK version 4
 * Ansible version 2.3

Role Variables
--------------

| Name                      | Default value         | Description                                                       |
|---------------------------|-----------------------|-------------------------------------------------------------------|
| mac_pool_allow_duplicates | UNDEF                 | If (true) allow a MAC address to be used multiple times in a pool. Default value is set by oVirt engine to false. |
| mac_pool_name             | UNDEF                 | Name of the the MAC pool to manage.                               |
| mac_pool_ranges           | UNDEF                 | List of MAC ranges. The from and to should be splitted by comma. For example: 00:1a:4a:16:01:51,00:1a:4a:16:01:61 |

Dependencies
------------

No.

Example Playbook
----------------

```yaml
- name: oVirt set mac pool
  hosts: localhost
  connection: local
  gather_facts: false

  vars:
    mac_pool_name: my_mac_pool
    mac_pool_allow_duplicates: false
    mac_pool_ranges:
      - 00:1a:4a:16:01:51,00:1a:4a:16:01:61

  roles:
    - ovirt-mac-pools
```

License
-------

Apache License 2.0
