mablanco.repomgmt
=

Ansible role to manage the software repositories of a Linux system of the following flavours: Debian, Ubuntu, RHEL and CentOS. It sanitizes the base repos and adds several well-known extra repos, if required by the user through a set of variables.

Role Variables
-

The following variables control whether an extra repo is installed (*true*) or not (*false*). 

####Debian extra repos
- **debian_multimedia**
- **debian_dotdeb**
- **debian_nginx**
- **debian_mariadb**

####Ubuntu extra repos
- **ubuntu_universe**
- **ubuntu_multiverse**
- **ubuntu_backports**
- **ubuntu_partners**
- **ubuntu_nginx**
- **ubuntu_mariadb**

Example Playbook
-

Example of how to use this role:

    - hosts: debian_servers
      vars:
         debian_nginx: true
         debian_dotdeb: true
      roles:
         - { role: mablanco.repomgmt }

TODO
-
- Ubuntu repos
- RHEL repos
- CentOS repos

License
-

GPLv3

Author Information
-

Marco Antonio Blanco <mablanco@correolibre.eu>
