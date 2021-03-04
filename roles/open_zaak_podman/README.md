Open Zaak (on a VM with Podman)
===============================

Deploy Open Zaak using [Podman](https://podman.io/).

Tested on RHEL 8.3.

Requirements
------------

- A PostgreSQL database
- Podman installed and configured for rootless containers (see also
  https://github.com/containers/podman/tree/v2.2.1-rhel/contrib/rootless-cni-infra)
- `python3-devel` or equivalent package and dev tools (requires `gcc`)
- the [`containers.podman`](https://galaxy.ansible.com/containers/podman) collection:
  ```bash
  $ ansible-galaxy collection install containers.podman
  ```

Note that this role conflicts with the `open_zaak_docker` role!

Role Variables
--------------

### SSL/NGINX

See the [`open_zaak_nginx` role documentation](../open_zaak_nginx/README.md).

### Environment variables

You can specify extra environment variables to include, for example to enable the
CMIS integration for the documents API.

```yaml
openzaak_extra_env:
  - name: CMIS_ENABLED
    value: "no"
```

Any entry in this list is added in order to the bottom of the `.env` file being
templated out.

Alternatively, you can override `openzaak_env_template` and specify a different `.env`
template file alltogether.

### Other

See `./defaults/main.yml` for the remainder.

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: app-servers
  roles:
    - role: geerlingguy.postgresql
      tags:
        - db

    - role: geerlingguy.certbot

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

    - role: open_zaak_podman
      vars:
        openzaak_version: '1.3.4'  # see https://hub.docker.com/r/openzaak/open-zaak/tags
      tags:
        - replicas

    - role: nginxinc.nginx
      vars:
        nginx_http_template:
          default:
            # set by open_zaak_podman role
            template_file: "{{ openzaak_nginx_template }}"
            conf_file_name: openzaak.conf
            conf_file_location: /etc/nginx/conf.d/
      tags:
        - replicas
```

License
-------

EUPL-1.2
