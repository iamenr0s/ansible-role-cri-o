Ansible Role: CRI-O
=========

This Ansible Role automates the installation of CRI-O on Linux systems.

Supported OSs
------------

- RockyLinux 9
- AlmaLinux 9
- Fedora 39

Requirements
------------

None.

Role Variables
--------------

Available variables and their default values are listed below (refer to defaults/main.yml):

# Package version.
	crio_version: "v1.29"

# Package options.
	crio_package: cri-o
	crio_package_state: present

# Service options.
	crio_service_state: started
	crio_service_enabled: true

# Storage driver
	crio_storage_driver: "overlay"

# Systemd slice to run the cri-o service in
	crio_systemd_slice: "system.slice"

# Container runtime
	crio_runtime: "runc"

# CRI-O extra configurations
	crio_extra_config: ''

Dependencies
------------

No dependencies required.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: iamenr0s.cri_o }

License
-------

BSD

Author Information
------------------


