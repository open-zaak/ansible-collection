Open Zaak (on a VM with Podman)
===============================

Deploy Open Zaak using [Podman](https://podman.io/).

Tested on RHEL 8.3.

Requirements
------------

- A PostgreSQL database
- Podman installed and configured for rootless containers
- `python3-devel` or equivalent package and dev tools (requires `gcc`)
- the [`containers.podman`](https://galaxy.ansible.com/containers/podman) collection:
  ```bash
  $ ansible-galaxy collection install containers.podman
  ```
- systemd (optional)

Note that this role conflicts with the `open_zaak_docker` role!

Role Variables
--------------

### SSL related

```yaml
openzaak_ssl: yes
openzaak_nginx_proto_value: '$scheme'
```

`openzaak_ssl` indicates whether Open Zaak is supposedly accessed over HTTPS (only).
Publicly accessible Open Zaak installations may not be exposed over plain HTTP.

`openzaak_nginx_proto_value` by default looks at the connection scheme of the client.
However, in configurations where SSL termination is performed _before_ nginx (because of
multiple reverse proxies, for example), you should set this to `'"https"'` otherwise
Open Zaak is incorrectly told it's accessed over `http` rather than `https`.

#### TLS certificates

When `openzaak_ssl` is `true`, the paths to the TLS key and certificate chain must be
provided. You can use your own certificates, or make use of Let's Encrypt to obtain
SSL certificates.

**Let's Encrypt**

The `openzaak_letsencrypt_enabled` variable is available. When enabled, the key and
certificate will be sourced from `/etc/letsencrypt/live/{{ openzaak_domain }}`.

The variable is `true` by default if the variable `certbot_certs` is defined (from the
`geerlingguy.certbot` role). If you're not using this role, you can specify it
explicitly:

```yaml
openzaak_letsencrypt_enabled: yes
```

**Own certificates**

You must explicitly specify the paths:

```yaml
openzaak_nginx_ssl_certificate: /path/to/ssl-cert-chain.pem
openzaak_nginx_ssl_key: /path/to/ssl-key.pem
```

Note that the certificate chain must include the certificate + all intermediate
certificates. The first certificate must correspond with the private key.

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
