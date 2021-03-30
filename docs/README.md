# Open Zaak deployment collection

This Ansible collection provides [Ansible][Ansible] roles to deploy apps in the Open
Zaak ecosystem.

Under the Open Zaak ecosystem, we recognize Open Zaak itself, Open Notificaties, but
also any other app that can run as a Docker container using a (PostgreSQL) database
and (Docker) volumes for persistent storage.

There are two main targets of deployment environment types:

* Kubernetes cluster(s)
* "Single server" deployments, where the machines are dedicated servers, VMs or other.

## Kubernetes

The roles with the `*_k8s` suffix are aimed at deploying on Kubernetes.

The Kubernetes resources used are all in vanilla Kubernetes installations, such as
deployments, services, ingresses and persistent volume (claims).

## Single server

Deployment on (physical) servers typically requires the following actions:

* installation of a PostgreSQL cluster
* installation of the Docker daemon/runtime
* deploying the application containers

The roles in this collection are aimed at preparing the server so that we can use
roles from the Ansible Galaxy (in particular, `geerlingguy` has excellent roles) to
do the actual installations.

Role overview, in logical order.

- `os_setup`: invokes the OS-specific `*_setup` role
    - `debian_setup`: tighten TLS security parameters, set up PostgreSQL package repository
    - `redhat_setup`: set up PostgreSQL package repo, set up Docker package repo
    - `suse_docker`: set up Docker package repo and install Docker
    - `suse_postgresql`: set up PostgreSQL package repo and install PostgreSQL

- `app_database`: provisions the application database(s). Requires PostgreSQL to be
  installed and running.

- application roles:
    - `nlx_docker`: run NLX as Docker container(s)
    - `open_notificaties_docker`: deploy Open Notificaties as containers
    - `open_zaak_docker`: deploy Open Zaak as containers

Example playbooks can be found in the [Open Zaak][Open Zaak] and
[Open Notificaties][Open Notificaties] repositories.

[Ansible]: https://docs.ansible.com/
[Open Zaak]: https://github.com/open-zaak/open-zaak/tree/main/deployment
[Open Notificaties]: https://github.com/open-zaak/open-notificaties/tree/main/deployment

## OS-specific notes

Some extra information or warnings are available per operating system flavour.

- [Red Hat](./RedHat.md)
