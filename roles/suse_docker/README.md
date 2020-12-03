Docker runtime on SUSE Linux
============================

Install the Docker runtime on Suse Linux.

It is recommended to rather use the `os_setup` role in this collection, which will
invoke this role.

Requirements
------------

On Suse Linux Enterprise Server (SESL), you need to enable the container module
using YaST or `SUSEConnect`.

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
    - role: suse_docker
```

License
-------

EUPL-1.2
