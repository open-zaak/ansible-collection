# CentOS (and Red Hat) hosts

Tested on CentOS 7

## Manual preparation

The following are pre-requisites for CentOS systems before the Ansible playbooks can run.

```shell
# yum update
```

### Hosts file configuration

The `ansible_python_interpreter` must be set to `/usr/bin/python2`

## PostgreSQL

CentOS 7 has PostgreSQL 9.2 in its repositories, which is not supported.

Therefore, we install the official PostgreSQL repositories and install a recent Postgres
version (11 by default, configurable through the `postgresql_version` variable).

## Docker

Centos 7 has Docker 1.13 in its repositories, which is fairly outdated.
