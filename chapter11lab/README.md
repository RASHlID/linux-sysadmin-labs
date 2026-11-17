# Chapter 11 — Working with Systemd

## RHCSA Linux System Administration Labs

This chapter focuses on managing Linux services using **Systemd** on Red Hat Enterprise Linux (RHEL).

The main objectives are to understand how services are managed, how Systemd unit files work, how dependencies affect services, and how to customise service behaviour using override files.

## 1. Understanding Systemd

Systemd is the system and service manager used by RHEL. It is responsible for starting and managing services and other system units.

`systemctl` is the main command used to interact with Systemd.

```bash
systemctl status sshd
systemctl list-units --type=service
systemctl list-unit-files --type=service
```

## 2. Managing Services

Common commands for controlling services:

```bash
sudo systemctl start sshd
sudo systemctl stop sshd
sudo systemctl restart sshd
sudo systemctl reload sshd
systemctl status sshd
```

To manage services at boot:

```bash
sudo systemctl enable sshd
sudo systemctl disable sshd
systemctl is-enabled sshd
```

To enable and start a service immediately:

```bash
sudo systemctl enable --now sshd
```

**Key learning:**

- `start` starts a service immediately.
- `stop` stops a running service.
- `restart` stops and starts a service.
- `reload` reloads configuration if supported.
- `enable` configures a service to start automatically at boot.
- `disable` removes automatic startup configuration.
- `enable --now` enables and starts the service.

Enabling a service does not necessarily start it immediately.

## 3. Installing and Managing vsftpd

`vsftpd` is an FTP server used for file transfers.

Install the package:

```bash
sudo dnf install -y vsftpd
```

Check its status:

```bash
systemctl status vsftpd
```

Start and enable the service:

```bash
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
```

Verify:

```bash
systemctl is-active vsftpd
systemctl is-enabled vsftpd
```

Expected results when the service is running and enabled:

```text
active
enabled
```

## 4. Understanding Systemd Unit Files

Systemd uses unit files to define how services and other units behave.

Common unit types include:

- `.service` — System services
- `.socket` — Socket activation
- `.target` — Groups of units
- `.mount` — Filesystem mounts
- `.timer` — Scheduled activation
- `.swap` — Swap devices or files

Important unit file locations:

```text
/usr/lib/systemd/system/
/etc/systemd/system/
/run/systemd/system/
```

- `/usr/lib/systemd/system/` contains vendor-supplied unit files.
- `/etc/systemd/system/` contains administrator configuration and overrides.
- `/run/systemd/system/` contains runtime unit configuration.

Inspect a unit file:

```bash
systemctl cat httpd.service
```

View detailed properties:

```bash
systemctl show httpd.service
```

A typical service file contains three sections:

```ini
[Unit]
Description=Example Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/example

[Install]
WantedBy=multi-user.target
```

**Key learning:**

- `[Unit]` defines descriptions and dependencies.
- `[Service]` defines how the service runs.
- `[Install]` defines installation and enablement information.

## 5. Understanding Systemd Dependencies

Dependencies determine relationships between Systemd units.

Important directives:

| Directive | Purpose |
|---|---|
| `Wants=` | Weak dependency; attempts to start another unit |
| `Requires=` | Stronger dependency; required unit failure can affect startup |
| `After=` | Defines startup ordering |
| `Before=` | Defines startup ordering in the opposite direction |

Example:

```ini
[Unit]
Wants=sshd.service
After=sshd.service
```

This requests that SSH start when the current unit starts, with the current unit ordered after SSH.

**Important:** `After=` alone does not start another service. It only controls ordering.

View dependencies:

```bash
systemctl list-dependencies vsftpd.service
```

View reverse dependencies:

```bash
systemctl list-dependencies --reverse vsftpd.service
```

## 6. Installing and Inspecting Apache

Apache HTTP Server is provided by the `httpd` package on RHEL.

Install Apache:

```bash
sudo dnf install -y httpd
```

Inspect its unit configuration:

```bash
systemctl cat httpd.service
```

Check service status:

```bash
systemctl status httpd
```

Start Apache:

```bash
sudo systemctl start httpd
```

## 7. Customising Services with Systemd Overrides

Vendor-supplied unit files should not normally be edited directly because package updates may overwrite changes.

Instead, use a Systemd override.

Configure Vim as the editor for the current command:

```bash
sudo env SYSTEMD_EDITOR=/usr/bin/vim systemctl edit httpd.service
```

For the Apache exercise, add:

```ini
[Service]
Restart=always
RestartSec=5s
```

This configures Systemd to restart Apache after qualifying service exits, with a five-second delay.

The override is stored at:

```text
/etc/systemd/system/httpd.service.d/override.conf
```

View the combined unit configuration:

```bash
systemctl cat httpd.service
```

Reload the Systemd manager configuration if required:

```bash
sudo systemctl daemon-reload
```

**Key learning:**

- `systemctl edit` creates or edits a service override.
- `Restart=always` configures automatic restarts, except for cases such as an explicit `systemctl stop`.
- `RestartSec=5s` sets the restart delay.
- `daemon-reload` makes Systemd reread unit configuration.

## 8. Testing Automatic Service Restart

Start Apache:

```bash
sudo systemctl start httpd
```

Verify:

```bash
systemctl status httpd
```

Simulate an unexpected failure:

```bash
sudo killall -9 httpd
```

Check the service again after the configured restart delay:

```bash
systemctl status httpd
```

The expected result is that Systemd restarts Apache automatically.

This demonstrates how Systemd can recover a service following an unexpected failure.

## 9. Chapter 11 —
