Open Notificaties (on a VM with Docker)
=======================================

Deploy Open Notificaties using Docker.

Requirements
------------

- A PostgreSQL database
- Docker runtime on the target machine

Role Variables
--------------

### SSL related

```yaml
opennotificaties_ssl: yes
opennotificaties_nginx_proto_value: '$scheme'
```

`opennotificaties_ssl` indicates whether Open Zaak is supposedly accessed over HTTPS (only).
Publicly accessible Open Zaak installations may not be exposed over plain HTTP.

`opennotificaties_nginx_proto_value` by default looks at the connection scheme of the client.
However, in configurations where SSL termination is performed _before_ nginx (because of
multiple reverse proxies, for example), you should set this to `'"https"'` otherwise
Open Zaak is incorrectly told it's accessed over `http` rather than `https`.

#### TLS certificates

When `opennotificaties_ssl` is `true`, the paths to the TLS key and certificate chain
must be provided. You can use your own certificates, or make use of Let's Encrypt to
obtain SSL certificates.

**Let's Encrypt**

The `opennotificaties_letsencrypt_enabled` variable is available. When enabled, the key
and certificate will be sourced from `/etc/letsencrypt/live/{{ opennotificaties_domain }}`.

The variable is `true` by default if the variable `certbot_certs` is defined (from the
`geerlingguy.certbot` role). If you're not using this role, you can specify it
explicitly:

```yaml
opennotificaties_letsencrypt_enabled: yes
```

**Own certificates**

You must explicitly specify the paths:

```yaml
opennotificaties_nginx_ssl_certificate: /path/to/ssl-cert-chain.pem
opennotificaties_nginx_ssl_key: /path/to/ssl-key.pem
```

Note that the certificate chain must include the certificate + all intermediate
certificates. The first certificate must correspond with the private key.

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
