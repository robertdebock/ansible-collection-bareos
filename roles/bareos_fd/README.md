# [bareos_fd](#bareos_fd)

Install and configure [BareOS](https://www.bareos.com/) File Daemon.

|GitHub|GitLab|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![github](https://github.com/robertdebock/ansible-role-bareos_fd/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bareos_fd/actions)|[![gitlab](https://gitlab.com/robertdebock-iac/ansible-role-bareos_fd/badges/master/pipeline.svg)](https://gitlab.com/robertdebock-iac/ansible-role-bareos_fd)|[![quality](https://img.shields.io/ansible/quality/63103)](https://galaxy.ansible.com/robertdebock/bareos_fd)|[![downloads](https://img.shields.io/ansible/role/d/63103)](https://galaxy.ansible.com/robertdebock/bareos_fd)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-bareos_fd.svg)](https://github.com/robertdebock/ansible-role-bareos_fd/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/robertdebock/ansible-role-bareos_fd/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.roles.bareos_fd
      bareos_fd_backup_configurations: yes
      bareos_fd_directors:
        - name: "bareos-dir"
          password: "secretpassword"
          monitor: no
          connection_from_client_to_director: yes
          connection_from_director_to_client: no
          tls_enable: yes
          tls_verify_peer: no
        - name: "disabled-director"
          enabled: no
      bareos_fd_messages:
        - name: "Standard"
          director:
            server: bareos-dir
            messages:
              - all
              - "!skipped"
              - "!restored"
          description: "Send relevant messages to the Director."
          append:
            file: "/var/log/bareos/bareos.log"
            messages:
              - all
              - "!skipped"
              - "!terminate"
          console:
            - all
            - "!skipped"
            - "!saved"
        - name: "disabled-message"
          enabled: no
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/robertdebock/ansible-role-bareos_fd/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.roles.bootstrap
    - role: robertdebock.roles.bareos_repository
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/robertdebock/ansible-role-bareos_fd/blob/master/defaults/main.yml):

```yaml
---
# defaults file for bareos_fd

# The client has these configuration parameters.
bareos_fd_backup_configurations: no
bareos_fd_hostname: "{{ inventory_hostname }}"
bareos_fd_max_job_bandwidth: "10 mb/s"
bareos_fd_message: "Standard"
bareos_fd_tls_enable: yes
bareos_fd_tls_verify_peer: no
bareos_fd_heartbeat_interval: 0

bareos_fd_directors: []
bareos_fd_messages: []
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/robertdebock/ansible-role-bareos_fd/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[robertdebock.bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap)|[![Build Status GitHub](https://github.com/robertdebock/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/robertdebock-iac/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/robertdebock-iac/ansible-role-bootstrap)|
|[robertdebock.bareos_repository](https://galaxy.ansible.com/robertdebock/bareos_repository)|[![Build Status GitHub](https://github.com/robertdebock/ansible-role-bareos_repository/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bareos_repository/actions)|[![Build Status GitLab](https://gitlab.com/robertdebock-iac/ansible-role-bareos_repository/badges/master/pipeline.svg)](https://gitlab.com/robertdebock-iac/ansible-role-bareos_repository)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/ansible-role-bareos_fd/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|[Debian](https://hub.docker.com/repository/docker/robertdebock/debian/general)|bookworm, bullseye, buster|
|[EL](https://hub.docker.com/repository/docker/robertdebock/enterpriselinux/general)|7, 8, 9|
|[Fedora](https://hub.docker.com/repository/docker/robertdebock/fedora/general)|36, 37|
|[opensuse](https://hub.docker.com/repository/docker/robertdebock/opensuse/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/robertdebock/ubuntu/general)|jammy, focal|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-bareos_fd/issues)

## [License](#license)

[Apache-2.0](https://github.com/robertdebock/ansible-role-bareos_fd/blob/master/LICENSE).

## [Author Information](#author-information)

[robertdebock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
