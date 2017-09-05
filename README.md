# mablanco.repomgmt

Ansible role to manage the software repositories of a Linux system of the following flavours: Debian, Ubuntu, RHEL, CentOS and SuSE. It sanitizes the base repos and adds several well-known extra repos if required by the user through a set of variables.

## Role Variables

### Extra repos
The following variables control whether an extra repo is installed (*true*) or not (*false*).

#### Debian
- **debian_multimedia**
- **debian_dotdeb**
- **debian_nginx**
- **debian_mariadb**

#### Ubuntu
- **ubuntu_universe**
- **ubuntu_multiverse**
- **ubuntu_backports**
- **ubuntu_partners**
- **ubuntu_nginx**
- **ubuntu_mariadb**

#### RHEL/CentOS
- **rhel_epel**
- **rhel_remi**
- **rhel_rpmfusion**
- **rhel_nux**
- **rhel_ius**

#### SuSE
pending

## Example Playbook

Example of how to use this role:

    - hosts: debian_servers
      vars:
         debian_nginx: true
         debian_dotdeb: true
      roles:
         - { role: mablanco.repomgmt }

## License

GPLv3
