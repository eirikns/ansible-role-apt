# Ansible Role: apt

An Ansible role to configure apt on Debian and Ubuntu systems. It handles package upgrades, installation of additional packages, and configuration of unattended upgrades and periodic apt tasks.

The role is tested against the latest Ubuntu LTS and Debian releases but may work with other Debian-derived distributions.

## Role Variables

See [`defaults/main.yaml`](defaults/main.yaml) for the full list of variables and their defaults. The most important ones are described below.

```yaml
apt_upgrade_packages: false
```

Whether to upgrade all installed packages to the latest version during role execution. When enabled, the role checks for `/var/run/reboot-required` after the upgrade and reboots if necessary. Set `apt_reboot_if_required: false` to skip the reboot — useful when `unattended-upgrades` is configured to handle reboots on its own schedule.

```yaml
apt_autoremove: false
```

Whether to run `apt autoremove` to remove packages that are no longer needed. Can be used independently of `apt_upgrade_packages`.

```yaml
apt_additional_packages:
  - curl
  - unzip
```

A list of additional packages to install via apt. Defaults to an empty list.

```yaml
apt_configure_unattended_upgrades: true
```

Whether to install and configure `unattended-upgrades`. When enabled, the role installs the package, ensures the service is running, and writes `/etc/apt/apt.conf.d/50unattended-upgrades`. Key settings you may want to override:

- `apt_unattended_upgrades_automatic_reboot` — whether to reboot automatically when required (default: `true`)
- `apt_unattended_upgrades_automatic_reboot_time` — time of day for automatic reboots (default: `"02:00"`)
- `apt_unattended_upgrades_mail` — email address for upgrade notifications; leave empty to disable (default: `""`)
- `apt_unattended_upgrades_mail_only_on_error` — only send email on errors, not on every upgrade (default: `false`)
- `apt_unattended_upgrades_dpkg_options` — dpkg options for handling config file conflicts (e.g. `["--force-confdef", "--force-confold"]`; default: `[]`)
- `apt_unattended_upgrades_origins_extra` — additional origins beyond the default security origins (default: `[]`)
- `apt_unattended_upgrades_package_blocklist` — packages to exclude from unattended upgrades (default: `[]`)

```yaml
apt_configure_periodic: true
```

Whether to configure periodic apt tasks via `/etc/apt/apt.conf.d/10periodic`. Controls how often package lists are updated and unattended upgrades are run.

## Example Playbook

```yaml
- hosts: servers
  roles:
    - role: apt
      vars:
        apt_additional_packages:
          - curl
          - unzip
          - htop
        apt_upgrade_packages: true
        apt_configure_unattended_upgrades: true
```

# License

[MIT No Attribution](https://opensource.org/license/mit-0)

# Author

This Ansible role was created in 2025 by Eirik Nicolai Synnes and published as an Open Source project in 2026.

# Acknowledgements

This role uses Docker images created by Jeff Geerling for testing. The approach for developing this role, as well how to use GitHub Actions to manage it, is inspired by his work. You can find him on GitHub at https://github.com/geerlingguy.
