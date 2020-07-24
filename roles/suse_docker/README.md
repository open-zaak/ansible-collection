Docker runtime on SUSE Linux
============================

Install the Docker runtime on Suse Linux.

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
