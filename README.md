# Ansible Role: Install X2GO

[![License][license-image]][license-url] [![Ansible Galaxy][ansible-galaxy-image]][ansible-galaxy-url]

Install [X2GO](https://wiki.x2go.org/doku.php) client\server\broker for Linux and Windows.

## Work on

### Ansible Galaxy style

```yaml
  platforms:
    - name: Ubuntu
      versions:
        - xenial
        - bionic
        - focal
    - name: Debian
      version:
        - jessie
        - stretch
        - buster
        - oldstable
        - stable
        - testing
    - name: EL (CentOS)
      versions:
        - 7
        - 8
    - name: opensuse
      vesrion:
        - tumbleweed
    - name: windows
      version:
        - 2008x64 (7 64bit)
        - 2008x86 (7 32bit)
        - 2019 (10 64bit)
```

## Requirements

None,

## Role Variables

```yaml
#--- Version section ---#

x2go_repo_version: release
# x2go_repo_version: nightly
# x2go_repo_version: esr

#--- Component section ---#

x2go_component: client
# x2go_component: server
# x2go_component: broker

x2go_client_win_setup_url: https://code.x2go.org/releases/X2GoClient_latest_mswin32-setup.exe

#--- Display manager section ---#

x2go_install_display_manager: true
# x2go_install_display_manager: false

x2go_default_display_manager: lightdm

#--- Window manager section ---#

x2go_install_window_manager: true
# x2go_install_window_manager: false

x2go_default_window_manager: icewm

#--- Printer section ---#

# https://stackoverflow.com/a/47994723

x2go_printer_support: true
# x2go_printer_support: false

# https://askubuntu.com/q/345083/457538
x2go_printer_disable_automatic_remote_installation: true

# http://www.columbia.edu/~em36/pdftoprinter.html
x2go_printer_support_backend: pdftoprinter

x2go_printer_support_backend_download_url: http://www.columbia.edu/~em36/PDFtoPrinter.exe

#--- CIFS section ---#

# For mount CIFS shares from x2go server.
# https://unix.stackexchange.com/a/34793

x2go_mount_cifs_support: true
# x2go_mount_cifs_support: false

x2go_mount_cifs_unprivileged_user: true
# x2go_mount_cifs_unprivileged_user: false

#--- Repo section ---#

x2go_repo_deb_key: https://packages.x2go.org/pub.key

x2go_repo_rpm_key: https://packages.x2go.org/pub.key

# If you *NOT* use apt-cacher-ng or other caching proxy - select "https".
http_or_https: http
# http_or_https: https
```

## Dependencies

### If you want deploy x2go client to Windows 7

Download and install [Windows Management Framework 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)

## HowTo

Quick config WinRM for Windows: <https://ru.stackoverflow.com/a/949971/191416>

## Example Playbooks

### I

Install latest stable x2go **client** on Windows or Linux over package manager of you distro with [pdftoprinter](https://stackoverflow.com/a/47994723):

`install-x2go-client.yml`:

```yaml
- name: Install x2go client
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - ansible-role-install-x2go
  tasks:
```

### II

Install x2go **server** stable version on Linux with `lightdm`+`icewm`+`mount.cifs` over package manager of you distro:

`install-x2go-server.yml`:

```yaml
- name: Install x2go server
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - ansible-role-install-x2go
  vars:
    x2go_component: server
  tasks:
```

### III

Install x2go 1 server and many clients:

`install-x2go.yml`:

```yaml
- name: Install x2go
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - ansible-role-install-x2go
  tasks:
```

`x2go-inventory.ini`:

```ini
[x2go-server]
172.16.10.10

[x2go-clients-linux]
10.10.10.5
10.10.10.6
10.10.10.7
10.10.10.8
10.10.10.9

[x2go-clients-windows]
10.10.10.20
10.10.10.21
10.10.10.22
10.10.10.23
10.10.10.24

[x2go-server:vars]
x2go_component=server
```

```bash
ansible-playbook -i ./x2go-inventory.ini ./install-x2go.yml
```

## License

Apache License, Version 2.0

## Author Information

[don Rumata](https://github.com/don-rumata)

## TODO

- Add tests.
- Add support config x2go broker.
- Add support more display managers and windows managers.

[license-image]: https://img.shields.io/github/license/don-rumata/ansible-role-install-x2go.svg
[license-url]: https://opensource.org/licenses/Apache-2.0

[ansible-galaxy-image]: https://img.shields.io/badge/ansible_galaxy-don__rumata.ansible__role__install__x2go-blue.svg
[ansible-galaxy-url]: https://galaxy.ansible.com/don_rumata/ansible_role_install_x2go
