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

The Ansible role `geerlingguy.docker` installs Docker from CentOS repositories. This
*may* have implications for Redhat support since you are using a different channel
for the package installation. RedHat itself will probably give no official support
on this.

At the time of writing this documentation, Docker CE 19.03 is installed.
