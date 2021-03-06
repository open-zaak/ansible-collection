NLX installation
================

Run NLX inways and outways with Docker, with transaction logging enabled.

Requirements
------------

You must have the Docker runtime on your target machine. See `geerlingguy.docker` for
example.

Follow the [NLX docs](https://docs.nlx.io) on how to obtain certificates and private
keys to access the NLX network.

You will need:

- the root certificate
- your own organization certificate
- your own organization private key

Additionally, with logging enabled you need a PostgreSQL database and user to write/read
from the database.

Role Variables
--------------

All default values of variables point to the demo network that can be accessed with
self-generated certificates.

### Certificates

```yaml
nlx_root_cert: "{{ lookup('url', 'https://certportal.demo.nlx.io/root.crt') }}"

# Follow https://docs.nlx.io/try-nlx/retrieve-a-demo-certificate/
nlx_inway_key: "PROVIDE THIS YOURSELF"
nlx_inway_cert: "PROVIDE THIS YOURSELF"
```

### Inway

```yaml
nlx_inway_version: latest
nlx_inway_image: nlxio/inway:{{ nlx_inway_version }}
nlx_inway_replicas:
  - nlx-inway-0
nlx_inway_directory_registration_address: directory-registration-api.demo.nlx.io:443
nlx_inway_self_address: inway.gemeente.nl:8443
# Service config - which services are reachable through NLX?
nlx_inway_services: []
# Example:
# nlx_inway_services:
#   - name: zaken
#     endpoint_url: https://open-zaak.gemeente.nl/zaken/api/v1
#     documentation_url = "https://open-zaak.gemeente.nl"
#     authorization_model = "none"
#     api_specification_document_url = "https://open-zaak.gemeente.nl/zaken/api/v1/schema/openapi.json"

```

### Outway

```yaml
nlx_outway_version: latest
nlx_outway_image: nlxio/outway:{{ nlx_outway_version }}
nlx_outway_replicas:
  - nlx-outway-0
nlx_outway_directory_inspection_address: directory-inspection-api.demo.nlx.io:443
```

### Transaction log database

```yaml
nlx_txdb_migrations_image: nlxio/txlog-db:latest
nlx_txlog_db_host: /var/run/postgresql
nlx_txlog_db_port: 5432
nlx_txlog_db_name: txlog-db
nlx_txlog_db_username: nlx
nlx_txlog_db_password: nlx  # override me!

nlx_txlog_db_username_api: nlx-txlog-api
nlx_txlog_db_password_api: nlx  # override me!
nlx_txlog_db_username_writer: nlx-txlog-writer
nlx_txlog_db_password_writer: nlx  # override me!
```

Dependencies
------------

There are no hard dependencies, however this role works well with
`geerlingguy.postgresql` and `app_database` for transaction log DB provisioning.

Example Playbook
----------------

```yaml
- hosts: app-servers
  roles:
    - role: geerlingguy.docker
    - role: nlx_docker
      vars:
        nlx_txdb_migrations_image: scrumteamzgw/txlog-db:latest  # fix with custom users/roles
```

License
-------

EUPL-1.2
