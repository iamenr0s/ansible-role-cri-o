[![Molecule](https://github.com/iamenr0s/ansible-role-cri-o/actions/workflows/molecule.yml/badge.svg)](https://github.com/iamenr0s/ansible-role-cri-o/actions/workflows/molecule.yml) [![Release](https://github.com/iamenr0s/ansible-role-cri-o/actions/workflows/release.yml/badge.svg)](https://github.com/iamenr0s/ansible-role-cri-o/actions/workflows/release.yml) ![Ansible Role](https://img.shields.io/ansible/role/d/iamenr0s/ansible_role_cri-o) [![CodeFactor](https://www.codefactor.io/repository/github/iamenr0s/ansible-role-cri-o/badge)](https://www.codefactor.io/repository/github/iamenr0s/ansible-role-cri-o)

Ansible Role: CRI-O
=========

This Ansible Role automates the installation of CRI-O on Linux systems.

Supported OSs
------------

- AlmaLinux 8
- AlmaLinux 9
- Fedora 38
- Fedora 39
- RockyLinux 8
- RockyLinux 9

Requirements
------------

None.

Role Variables
--------------

Available variables and their default values are listed below (refer to defaults/main.yml):

#### Package version.
	crio_version: "v1.29"

#### Package options.
	crio_package: cri-o
	crio_package_state: present

#### Service options.
	crio_service_state: started
	crio_service_enabled: true

#### Storage driver
	crio_storage_driver: "overlay"

#### Systemd slice to run the cri-o service in
	crio_systemd_slice: "system.slice"

#### Container runtime
	crio_runtime: "runc"

#### CRI-O extra configurations
	crio_extra_config: ''

Dependencies
------------

No dependencies required.

Example Playbook
----------------

    - hosts: all
      roles:
         - { role: iamenr0s.ansible_role_cri-o }

License
-------

BSD

Author Information
------------------

