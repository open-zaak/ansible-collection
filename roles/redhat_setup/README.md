Red Hat setup
=============

This role prepares a Red Hat target machine for Docker-based deployments of the Open
Zaak ecosystem.

It is recommended to rather use the `os_setup` role in this collection, which will
invoke this role.

Requirements
------------

A Red Hat Enterprise Linux (RHEL) or CentOS based system.

The role has been tested against CentOS 7 and 8, and *should* work on RHEL 7/8.

In your inventory, ensure to specify the appropriate Python version, depending on the
OS version:

RHEL/CentOS 7

```
<host> ansible_python_interpreter=/usr/bin/python2
```

Role Variables
--------------

None, but external variables affect the behaviour:

- `postgresql_version` - if defined, the PostgreSQL package repositories are installed

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: app-servers
  roles:
    - role: redhat_setup
```

License
-------

EUPL-1.2
