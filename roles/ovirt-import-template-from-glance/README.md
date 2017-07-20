# TODO
oVirt import template from glance
==================================

This role imports disk from glance as template, add nic to template and copy its disks to storage domains.

Requirements
------------

 * oVirt Python SDK version 4
 * Ansible version 2.3

Role Variables
--------------

The value of item in `external_templates_from_glance` list can contain following parameters:

| Name            | Default value  | Description                           |
|-----------------|----------------|---------------------------------------|
| name            | UNDEF          | Name of the the template to manage.   |
| image_provider  | UNDEF          | When state is imported this parameter specifies the name of the image provider to be used. |
| image_disk      | UNDEF          | When state is imported and image_provider is used this parameter specifies the name of disk to be imported as template. |
| storage_domain  | UNDEF          | When state is imported this parameter specifies the name of the destination data storage domain. |
| cluster         | UNDEF          | Name of the cluster, where template should be created/imported. |

More information about storages parameters can be found [here](http://docs.ansible.com/ansible/latest/ovirt_templates_module.html).


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
    external_templates_from_glance:
      - name: template_name
        state: imported
        image_provider: myglance
        image_disk: rhel7.4
        storage_domain: my_storage_domain
        cluster: my_cluster

    destination_domains:
      - my_storage_domain
      - my_storage_domain_2

  roles:
    - ovirt-import-template-from-glance
```

License
-------

Apache License 2.0
