Open Notificaties (on Kubernetes)
=================================

Deploy Open Notificaties in a Kubernetes cluster.

Requirements
------------

You need a Kubernetes cluster where you can create or have access to a namespace. Within
the namespace, you need to be able to create/modify deployments, services (load balancer
type), secrets and configmaps.

Additionally, you need a PostgreSQL database for the application data.

Role Variables
--------------

See `./defaults/main.yml`:

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: app-servers
  roles:
    - role: app_database
      vars:
        app_db_provision_user: no
        app_db_provision_database: no
        app_db_become_user: postgres

        app_db_host: ''
        app_db_port: "{{ opennotificaties_db_port }}"
        app_db_name: "{{ opennotificaties_db_name }}"
        app_db_extensions:
          - pg_trgm
      tags:
        - app_db

    - role: open_notificaties_k8s
      vars:
        opennotificaties_version: '1.1.0'  # see https://hub.docker.com/r/openzaak/open-notificaties/tags
```

License
-------

EUPL-1.2
