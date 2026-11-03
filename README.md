# mablanco.repomgmt

Ansible role to manage the software repositories of Debian, Ubuntu, EL, Fedora and openSUSE: the base repositories of Debian and Ubuntu, and several well-known extra repositories.

**Disclaimer:** the extra repositories are not official, so add them at your own risk. This role doesn't recommend them; it only makes adding them easier if you decide to.

## Supported platforms

Debian 12 and 13, Ubuntu 24.04 and 26.04, EL 9 and 10, Fedora 43 and 44, and openSUSE Leap 16.0, all tested in CI.

On EL and openSUSE, the role needs the `community.general` collection (`ansible-galaxy collection install community.general`).

## Base repositories (Debian and Ubuntu)

By default the role leaves the base repositories alone. With **repomgmt_manage_base** set to `true`, it writes them in `/etc/apt/sources.list.d/debian.sources` or `ubuntu.sources`, in the deb822 format that current releases use, and empties `/etc/apt/sources.list`.

| Variable | Default |
|---|---|
| **repomgmt_debian_mirror** | `http://deb.debian.org/debian` |
| **repomgmt_debian_security_mirror** | `http://deb.debian.org/debian-security` |
| **repomgmt_debian_components** | `[main, non-free-firmware]`, as Debian's installer. Add `contrib` and `non-free` if you need them. |
| **repomgmt_debian_backports** | `false` |
| **repomgmt_ubuntu_mirror** | `http://archive.ubuntu.com/ubuntu`, or `http://ports.ubuntu.com/ubuntu-ports` on architectures other than amd64 |
| **repomgmt_ubuntu_security_mirror** | `http://security.ubuntu.com/ubuntu`, or the ports mirror |
| **repomgmt_ubuntu_components** | `[main, restricted, universe, multiverse]`, as Ubuntu's installer |
| **repomgmt_ubuntu_backports** | `true`, as Ubuntu's installer |

## Extra repositories

Each variable adds a repository when `true`. They all default to `false`, and each one applies only to the platforms in its row.

| Variable | Repository | Platforms |
|---|---|---|
| **repomgmt_deb_multimedia** | [deb-multimedia](https://www.deb-multimedia.org/) | Debian |
| **repomgmt_nginx** | [nginx.org](https://nginx.org/en/linux_packages.html), stable packages | Debian, Ubuntu |
| **repomgmt_mariadb** | [MariaDB](https://mariadb.org/download/?t=repo-config), version in **repomgmt_mariadb_version** (default `11.8`, an LTS release) | Debian, Ubuntu |
| **repomgmt_sury** | [Ondřej Surý's PHP packages](https://deb.sury.org/): `packages.sury.org` on Debian, the `ondrej/php` PPA on Ubuntu | Debian, Ubuntu |
| **repomgmt_epel** | [EPEL](https://docs.fedoraproject.org/en-US/epel/), and CRB, which EPEL needs | EL |
| **repomgmt_remi** | [Remi](https://rpms.remirepo.net/), which enables `remi-safe`; enable the PHP version you want afterwards | EL, Fedora |
| **repomgmt_rpmfusion** | [RPM Fusion](https://rpmfusion.org/), free and nonfree | EL, Fedora |
| **repomgmt_packman** | [Packman](https://packman.links2linux.org/), with priority 90 | openSUSE Leap |

On EL, Remi and RPM Fusion depend on EPEL, so the role adds it with either of them. On RHEL, enable CRB yourself with `subscription-manager repos --enable codeready-builder-for-rhel-<version>-<arch>-rpms`.

The APT repositories are written in deb822 format, with their keys in `/etc/apt/keyrings`. The role doesn't check that a repository has packages for your release: at the time of writing, the `ondrej/php` PPA has none for Ubuntu 26.04. Its key is also RSA 1024, so `apt-get update` warns that it uses a weak algorithm.

## Example playbook

```yaml
- hosts: debian_servers
  become: true
  roles:
    - role: mablanco.repomgmt
      vars:
        repomgmt_manage_base: true
        repomgmt_debian_components: [main, contrib, non-free, non-free-firmware]
        repomgmt_nginx: true
        repomgmt_sury: true
```

## Upgrading from older versions

This version is a rewrite:

- The variables have the role's prefix, and some have new names:

  | Old | New |
  |---|---|
  | `debian_multimedia` | `repomgmt_deb_multimedia` |
  | `debian_nginx`, `ubuntu_nginx` | `repomgmt_nginx` |
  | `debian_mariadb`, `ubuntu_mariadb` | `repomgmt_mariadb` |
  | `debian_sury` | `repomgmt_sury` |
  | `ubuntu_universe`, `ubuntu_multiverse`, `ubuntu_backports` | `repomgmt_ubuntu_components` and `repomgmt_ubuntu_backports`, with `repomgmt_manage_base` |
  | `rhel_epel`, `rhel_remi`, `rhel_rpmfusion` | `repomgmt_epel`, `repomgmt_remi`, `repomgmt_rpmfusion` |
  | `sles_packman` | `repomgmt_packman` |

- The role no longer rewrites the base repositories unless you set `repomgmt_manage_base`.
- Repositories that no longer exist are gone: Dotdeb, Nux Dextop, Nux Misc, IUS, CentOS Plus and Contrib, and Ubuntu's partner repository. The role removes the files that older versions created for them, since they break `apt-get update`.
- When you enable a repository that older versions added as a `.list` file (`nginx.list`, `sury.list`, `multimedia.list`, `backports.list` and `ubuntu-*.list`), the role replaces it with its deb822 file. It doesn't remove the keys that older versions added with `apt-key`.
- Debian 11 and older, Ubuntu 22.04 and older, EL 8 and older, and SLES are no longer supported.

## Development

The role is tested with [Molecule](https://ansible.readthedocs.io/projects/molecule/) in Docker containers of every supported platform, and linted with [ansible-lint](https://ansible.readthedocs.io/projects/lint/). CI runs both on every change and every week.

```sh
pip install -r requirements-dev.txt
ansible-lint
molecule test
```

## License

GPL-3.0-only
