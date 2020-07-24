Open Zaak (on a VM with Docker)
===============================

Deploy Open Zaak using Docker.

Requirements
------------

- A PostgreSQL database
- Docker runtime on the target machine

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
    - role: debian_setup

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
        app_db_port: "{{ openzaak_db_port }}"
        app_db_name: "{{ openzaak_db_name }}"
        app_db_extensions:
          - pg_trgm
      tags:
        - app_db

    - role: open_zaak_docker
      vars:
        openzaak_version: '1.2.0'  # see https://hub.docker.com/r/openzaak/open-zaak/tags
      tags:
        - replicas

    - role: nginxinc.nginx
      vars:
        nginx_http_template:
          default:
            # set by open_zaak_docker role
            template_file: "{{ openzaak_nginx_template }}"
            conf_file_name: openzaak.conf
            conf_file_location: /etc/nginx/conf.d/
      tags:
        - replicas
```

License
-------

EUPL-1.2
