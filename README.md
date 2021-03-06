# mablanco.repomgmt

Ansible role to manage the software repositories of a Linux system of the following flavours: Debian, Ubuntu, RHEL, CentOS and SuSE. It sanitizes the base repos and adds several well-known extra repos if required by the user through a set of variables.

**Disclaimer:** As the extra repos are not official, install them at you own risk. This role is not explicitly recommending their use, it's just easing their installation in case you decide so.

## Role Variables

### Extra repos
The following variables control whether an extra repo is installed (*true*) or not (*false*).

#### Debian
- **debian_dotdeb** (Debian <= 8)
- **debian_mariadb**
- **debian_multimedia** (Debian >= 8)
- **debian_nginx**
- **debian_sury** (Debian >= 9)

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
- **rhel_nux_misc**
- **rhel_ius**

#### Fedora
- **rhel_remi**
- **rhel_rpmfusion**

#### SuSE
- **sles_packman**

## Example Playbook

Example of how to use this role:

    - hosts: debian_servers
      vars:
         debian_nginx: true
         debian_sury: true
      roles:
         - { role: mablanco.repomgmt }

## License

GPLv3
