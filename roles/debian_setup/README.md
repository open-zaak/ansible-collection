Debian setup
============

Prepare a Debian-based system for the `*_docker` roles and PostgreSQL server
installation.

It is recommended to rather use the `os_setup` role in this collection, which will
invoke this role.

Requirements
------------

A Debian or Ubuntu based system.

Role Variables
--------------

None

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: app-servers
  roles:
    - role: debian_setup
```

License
-------

EUPL-1.2
