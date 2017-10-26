oVirt Engine Setup
==================

Generate answerfile and run engine-setup with it. In case you pass variable
'ovirt_engine_update: True' also setup packages will be updated.

Target Systems
--------------

* engine

Requirements
------------

 * Preinstalled clean environment with configured repositories.
 * Ansible version 2.3

Role Variables
--------------

By default engine-setup uses answer file specific for version of oVirt,
based on ``ovirt_engine_version`` parameter. You can specify own answer file
as ``ovirt_engine_answer_file_path``.

| Name                            | Default value         |  Description                                              |
|---------------------------------|-----------------------|-----------------------------------------------------------|
| ovirt_engine_type               | ovirt-engine          | Type of product 'ovirt-engine' or 'rhevm'  |
| ovirt_engine_version            | UNDEF                 | Allowed version: [3.6, 4.0, 4.1] |
| ovirt_engine_dwh                | UNDEF                 | Bool value for installing local DWH. |
| ovirt_engine_answer_file_path   | UNDEF                 | /path/to/custom/answerfile. |
| ovirt_engine_db_host            | localhost             | IP or hostname of PostgreSQL server for engine database. |
| ovirt_engine_db_port            | 5432                  |  Server listening port. |
| ovirt_engine_db_name            | engine                | DB name for ovirt-engine. |
| ovirt_engine_db_user            | engine                |  DB user which can access ovirt-engine DB. |
| ovirt_engine_db_password        | UNDEF                 | password for user of ovirt-engine DB. |
| ovirt_engine_dwh_db_host        | localhost             | IP or hostname of PostgreSQL server for DWH database. |
| ovirt_engine_dwh_db_port        | 5432                  | Server listening port. |
| ovirt_engine_dwh_db_name        | ovirt_engine_history  |  DB name for ovirt-engine-dwh. |
| ovirt_engine_dwh_db_user        | ovirt_engine_history  |  DB user which can access ovirt-engine-dwh DB. |
| ovirt_engine_dwh_db_password    | UNDEF                 | password for user of ovirt-engine DB. |

* ISO domain related options:

| Name                              | Default value         |  Description                               |
|-----------------------------------|-----------------------|--------------------------------------------|
| ovirt_engine_configure_iso_domain | False                 | Whether to confiure ISO domain on engine.   |
| ovirt_engine_iso_domain_path      | /var/lib/exports/iso  | Create local ISO domain on engine machine.  |
| ovirt_engine_iso_domain_name      | IDO_DOMAIN            | Name of ISO domain.                         |
| ovirt_engine_iso_domain_acl       | 0.0.0.0/0.0.0.0(rw)   | ACL permissions for ISO domain mount point. |

* Configure firewall

| Name                             | Default value  |  Description |
|----------------------------------|----------------|---------------------------------------------------------------------------------------------------------------------|
|  ovirt_engine_firewall_manager   | firewalld      | In case you want to configure firewall, select the firewall manager 'firewalld', 'iptables', or put null otherwise. |

* Update

| Name                  | Default value         |  Description                                              |
|-----------------------|-----------------------|-----------------------------------------------------------|
| ovirt_engine_update   | False                 | True if you want to update setup packages and run engine-setup also when engine is already installed and running. |


Dependencies
------------

* ovirt-engine-install-packages

Example Playbook
----------------

```yaml
---
- hosts: engine
  vars:
    ovirt_engine_type: 'ovirt-engine'
    ovirt_engine_version: '4.1'
    ovirt_engine_organization: 'of.ovirt.engine.com'
    ovirt_engine_admin_password: 'secret'
  roles:
    - ovirt-engine-setup
```

Extra
-----

If you want to install just the `ovirt-engine` and configure the DWH database later on a different host, then the playbook would look like:

```yaml
---
- hosts: engine
  vars:
    ovirt_engine_type: 'ovirt-engine'
    ovirt_engine_version: '4.1'
    ovirt_rpm_repo: 'http://resources.ovirt.org/pub/yum-repo/ovirt-release41.rpm'
    ovirt_engine_organization: 'ovirt.org'
    ovirt_engine_admin_password: 'secret'
    ovirt_engine_dwh: False
  roles:
    - ovirt-common
    - ovirt-engine-install-packages
    - ovirt-engine-setup
```

Running this play file would be done like

```bash
$ ansible-playbook site.yml -i inventory
```
