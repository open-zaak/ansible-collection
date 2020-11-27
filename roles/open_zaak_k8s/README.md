Open Zaak (on Kubernetes)
=========================

Deploy Open Zaak in a Kubernetes cluster.

Requirements
------------

You need a Kubernetes cluster where you can create or have access to a namespace. Within
the namespace, you need to be able to create/modify deployments, services (load balancer
type), secrets and configmaps.

Additionally, you need a PostgreSQL database for the application data.

Role Variables
--------------

### Environment variables

You can specify extra environment variables to include, for example to enable the
CMIS integration for the documents API.

```yaml
openzaak_extra_env:
  - name: CMIS_ENABLED
    value: "no"
```

Any entry in this list is added to the deployment environment variables.

### Other role variables

See `./defaults/main.yml` for the remainder.

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
        app_db_port: "{{ openzaak_db_port }}"
        app_db_name: "{{ openzaak_db_name }}"
        app_db_extensions:
          - pg_trgm
      tags:
        - app_db

    - role: open_zaak_k8s
      vars:
        openzaak_version: '1.2.0'  # see https://hub.docker.com/r/openzaak/open-zaak/tags
```

License
-------

EUPL-1.2
