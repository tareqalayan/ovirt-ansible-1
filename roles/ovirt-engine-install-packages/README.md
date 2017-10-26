oVirt Engine Install Packages
======================

This role installs all necessary packages for oVirt engine deployment.

Target Systems
--------------

* engine


Requirements
------------

Preinstalled clean environment with configured repositories.


Role Variables
--------------

| Name                                        | Default value                                                         |                                                  |
|---------------------------------------------|-----------------------------------------------------------------------|--------------------------------------------------|
| ovirt_engine_install_packages_package_list  | [ovirt-engine,ovirt-engine-dwh-setup,ovirt-engine-extension-aaa-ldap] |  List of the packages to be installed on engine. |


Dependencies
------------

* ovirt-repositories


Example Playbook
----------------

```yaml
---
- hosts: engine
  roles:
    - ovirt-engine-install-packages
```
