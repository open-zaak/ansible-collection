OS setup
========

This role prepares the operating system of the target machine, by loading the OS-specific
setup role.

Currently the following OS families are supported:

- Debian/Ubuntu
- SLES/OpenSUSE
- Red Hat/CentOS

Support for other OS-flavours can be contributed of course.

Requirements
------------

A system with any of the supported operating systems, and root access to the system (
either as `root` user or with `sudo`).

Role Variables
--------------

* `os_setup_docker`: whether to install the Docker runtime. If enabled, then extra package
  repositories are prepared on SLES/OpenSUSE. Defaults to `yes`.

* `os_setup_db`: whether to install the database server. If enabled, then extra package
  repositories are prepared on SLES/OpenSUSE. Defaults to `yes`.

Other external variables affect the behaviour of the OS-specific roles.

- `postgresql_version` - if defined, the PostgreSQL package repositories are installed.
  Applies for Debian/Ubuntu and Red Hat.

Dependencies
------------

None

Exmaple Playbook
----------------

```yaml
- hosts: app-servers
  roles:
    - role: os_setup
```

License
-------

EUPL-1.2
