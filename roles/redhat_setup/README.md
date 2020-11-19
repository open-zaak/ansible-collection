Red Hat setup
=============

This role prepares a Red Hat target machine for Docker-based deployments of the Open
Zaak ecosystem.

Requirements
------------

A Red Hat Enterprise Linux (RHEL) or CentOS based system. RHEL/CentOS 7 are actively
tested, and the role *should* work on RHEL/CentOS 8.

Role Variables
--------------

None, but external variables affect the behaviour:

- `postgresql_version` - if defined, the PostgreSQL package repositories are installed

Dependencies
------------

None

Exmaple Playbook
----------------

```yaml
- hosts: app-servers
  roles:
    - role: redhat_setup
```

License
-------

EUPL-1.2
