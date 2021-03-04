Open Notificaties (on a VM with Docker)
=======================================

Deploy Open Notificaties using Docker.

Requirements
------------

- A PostgreSQL database
- Docker runtime on the target machine

Role Variables
--------------

### SSL/NGINX

See the [`open_notificaties_nginx` role documentation](../open_notificaties_nginx/README.md).

### Environment variables

You can specify extra environment variables to include, for example to enable the
CMIS integration for the documents API.

```yaml
opennotificaties_extra_env:
  - name: EMAIL_HOST
    value: "mail.example.com"
```

Any entry in this list is added in order to the bottom of the `.env` file being
templated out.

Alternatively, you can override `opennotificaties_env_template` and specify a different `.env`
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

    - role: geerlingguy.docker

    - role: geerlingguy.certbot

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

    - role: open_notificaties_docker
      vars:
        opennotificaties_version: '1.1.0'  # see https://hub.docker.com/r/openzaak/open-notificaties/tags
        opennotificaties_cache_db: 1
      tags:
        - replicas

    - role: nginxinc.nginx
      vars:
        nginx_http_template:
          default:
            # set by open_notificaties_docker role
            template_file: "{{ opennotificaties_nginx_template }}"
            conf_file_name: opennotificaties.conf
            conf_file_location: /etc/nginx/conf.d/
      tags:
        - replicas
```

License
-------

EUPL-1.2
