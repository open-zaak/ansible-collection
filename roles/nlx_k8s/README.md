NLX installation (on Kubernetes)
================================

Run NLX inways and outways on Kubernetes, with transaction logging enabled.

Requirements
------------

You need a Kubernetes cluster where you can create or have access to a namespace. Within
the namespace, you need to be able to create/modify deployments, services (load balancer
type), secrets and configmaps.

Follow the [NLX docs](https://docs.nlx.io) on how to obtain certificates and private
keys to access the NLX network.

You will need:

- the root certificate
- your own organization certificate
- your own organization private key

Additionally, with logging enabled you need a PostgreSQL database and user to write/read
from the database.

Make sure to add a DNS entry for the inway to the load balancer IP address.

Role Variables
--------------

All default values of variables point to the demo network that can be accessed with
self-generated certificates.

### Kubernetes

```yaml
nlx_namespace: nlx
nlx_instance: prod
```

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
nlx_inway_replicas: 1
nlx_inway_directory_registration_address: directory-registration-api.demo.nlx.io:443
nlx_inway_self_address: inway.gemeente.nl:8443
# Service config - which services are reachable through NLX?
nlx_inway_services: []
# Example:
# nlx_inway_services:
#   - name: zaken
#     endpoint_url: http://some-svc.some-namespace:80/zaken/api/v1
#     documentation_url = "https://open-zaak.gemeente.nl"
#     authorization_model = "none"
#     api_specification_document_url = "http://some-svc.some-namespace:80/zaken/api/v1/schema/openapi.json"

```

### Outway

```yaml
nlx_outway_version: latest
nlx_outway_image: nlxio/outway:{{ nlx_outway_version }}
nlx_outway_replicas: 1
nlw_outway_directory_inspection_address: directory-inspection-api.demo.nlx.io:443
```

### Transaction log database

```yaml
nlx_txdb_migrations_image: nlxio/txlog-db:latest
nlx_txlog_db_host: localhost
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

You can use the `app_database` role to provision the NLX transaction log database.

Example Playbook
----------------

```yaml
- hosts: app-servers
  roles:
    - role: nlx_k8s
      vars:
        nlx_txdb_migrations_image: scrumteamzgw/txlog-db:latest  # fix with custom users/roles
```

License
-------

EUPL-1.2
