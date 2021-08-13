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

- `openzaak_pod_security_context.fsgroup` (sets mounted volume permissions to this GID)
- `openzaak_service_account` (define service account to run the pod, leave blank to default back to "default")
- `openzaak_create_service_account` (true or false, requires cluster admin permissions. This will create a custom SA account with the value from "openzaak_service_account", rbac binding and SecurityContextConstraints(scc), currently this SCC allows a range of 1000-2000. Needed especially for openshift platforms, the default SA uses an SCC that does not allow low GID ranges. Contact your cluster administrator for more information.)

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
