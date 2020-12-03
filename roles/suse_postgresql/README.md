PostgreSQL on SUSE Linux
========================

Install the PostgreSQL database server on SUSE Linux, with postgis enabled.

Note that only PostgreSQL 10 is supported due to the unavailability of postgis packages
for newer versions.

It is recommended to rather use the `os_setup` role in this collection, which will
invoke this role.

Requirements
------------

None.

Role Variables
--------------

None.

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: app-servers
  roles:
    - role: suse_postgresql
```

License
-------

EUPL-1.2
