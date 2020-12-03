# CentOS (and Red Hat) hosts

Tested on CentOS 7 and CentOS 8.

## Manual preparation

The following are pre-requisites for CentOS systems before the Ansible playbooks can run.

RHEL/CentOS 7:

* Python 2 installation

RHEL/CentOS 8:

* Python 3 installation
* Python 3 pip installed: `dnf install python3-pip`

### Hosts file configuration

For RHEL/CentOS 7: the `ansible_python_interpreter` must be set to `/usr/bin/python2`

For RHEL/CentOS 8: the `ansible_python_interpreter` must be set to `/usr/bin/python3`

## PostgreSQL

CentOS 7 has PostgreSQL 9.2 in its repositories, which is not supported.

Therefore, we install the official PostgreSQL repositories and install a recent Postgres
version (11 by default, configurable through the `postgresql_version` variable).

CentOS 8 has PostgreSQL 10, but we still install the extra repositories to be able
to install more recent PostgreSQL and Postgis versions.

## Docker

Centos 7 has Docker 1.13 in its repositories, which is fairly outdated. CentOS 8 by
default does not appear to have Docker available.

The Ansible role `geerlingguy.docker` installs Docker from CentOS repositories. This
*may* have implications for Redhat support since you are using a different channel
for the package installation. RedHat itself will probably give no official support
on this.

At the time of writing this documentation, Docker CE 19.03 is installed.
