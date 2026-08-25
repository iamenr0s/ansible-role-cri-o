[![Molecule](https://github.com/iamenr0s/ansible-role-cri-o/actions/workflows/molecule.yml/badge.svg)](https://github.com/iamenr0s/ansible-role-cri-o/actions/workflows/molecule.yml) ![Ansible Role](https://img.shields.io/ansible/role/d/iamenr0s/ansible_role_cri_o) [![CodeFactor](https://www.codefactor.io/repository/github/iamenr0s/ansible-role-cri-o/badge)](https://www.codefactor.io/repository/github/iamenr0s/ansible-role-cri-o)

Ansible Role: CRI-O
====================

This Ansible Role automates the installation and configuration of [CRI-O](https://cri-o.io/) on RHEL-family, Fedora, Debian, and Ubuntu hosts.

Features
--------
- Adds the upstream OBS `cri-o` repository (yum/dnf or APT, matching `crio_version`).
- Installs and configures CRI-O, including its `/etc/crio/crio.conf.d` drop-in directory.
- Optionally installs and configures `crun` as the container runtime.
- Optionally pins the crio systemd service to a non-default systemd slice.
- Supports arbitrary extra CRI-O configuration via `crio_extra_config`.
- Manages the crio systemd service state.

Requirements
------------
- Ansible 2.9 or higher.
- No additional collections required.

Supported Platforms
--------------------

| Family     | Versions              |
| ---------- | ---------------------- |
| AlmaLinux  | 8, 9, 10                |
| RockyLinux | 8, 9, 10                |
| Fedora     | 42, 43, 44              |
| Debian     | 12 (bookworm), 13 (trixie)\* |
| Ubuntu     | 22.04 (jammy), 24.04 (noble) |

\* **Debian 13 (trixie) is a known-failing CI leg.** trixie's apt now verifies
signatures with `sqv`, which since 2026-02-01 rejects the legacy v3 OpenPGP
signature the upstream CRI-O OBS repository still signs its `InRelease` with.
This is an upstream signing issue, not a bug in this role — there is no
Ansible-side fix. The role and its APT repository setup work correctly on
trixie once upstream re-signs with a modern key; until then, `MOLECULE_DISTRO=debian13 molecule test` fails at the `apt-get update` step and CI runs
it with `continue-on-error: true`.

Role Variables
--------------

Available variables and their default values are listed below (refer to `defaults/main.yml`):

#### Package version
	crio_version: "v1.32"

#### Package options
	crio_package: cri-o
	crio_package_state: present

#### Service options
	crio_service_state: started
	crio_service_enabled: true

#### Storage driver
	crio_storage_driver: "overlay"

#### Systemd slice to run the cri-o service in
	crio_systemd_slice: "system.slice"

#### Container runtime
	crio_runtime: "runc"

#### CRI-O extra configuration
	crio_extra_config: ''

Example Playbook
----------------

```yaml
- hosts: all
  become: true
  roles:
    - role: iamenr0s.ansible_role_cri_o
```

Pin a specific CRI-O version and use `crun` as the runtime:

```yaml
- hosts: all
  become: true
  vars:
    crio_version: "v1.32"
    crio_runtime: crun
  roles:
    - role: iamenr0s.ansible_role_cri_o
```

Run the crio service in a dedicated systemd slice with extra configuration:

```yaml
- hosts: all
  become: true
  vars:
    crio_systemd_slice: "kubepods.slice"
    crio_extra_config: |
      [crio.runtime]
      log_level = "debug"
  roles:
    - role: iamenr0s.ansible_role_cri_o
```

Dependencies
------------
No dependencies required.

CI & Release (maintainers)
---------------------------
A single workflow (`.github/workflows/molecule.yml`) runs lint and the full Molecule distro matrix on pushes to `main`, PRs, and `v*` tags. On `v*` tags, a `release` job publishes to Ansible Galaxy after all tests pass.

The Galaxy API key lives in the `galaxy` GitHub environment, which only `v*` tags may target. One-time setup:

```bash
# Galaxy publishing key (environment-scoped, get it from galaxy.ansible.com/ui/token)
gh secret set GALAXY_API_KEY --env galaxy --repo iamenr0s/ansible-role-cri-o

# Code scanning notifications (Slack webhook URL; for Discord append /slack to the webhook URL)
gh secret set SECURITY_ALERT_WEBHOOK --env galaxy --repo iamenr0s/ansible-role-cri-o
```

`.github/workflows/code-scanning-notify.yml` polls the code-scanning API every 6 hours and posts new or updated open alerts to that webhook (GitHub Actions cannot trigger on `code_scanning_alert` directly).

To release: tag a commit `vX.Y.Z` and push the tag — CI gates the Galaxy publish.

## Contributing

Contributions are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md) for the
local pipeline commands and pull request checklist. This project follows the
[Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md).

## Security

See [SECURITY.md](SECURITY.md) — GitHub private vulnerability reporting, no
public issues for security bugs.

## License

This project is licensed under the [MIT License](LICENSE).

## Author Information

Author: iamenr0s

Galaxy: `iamenr0s.ansible_role_cri_o`
