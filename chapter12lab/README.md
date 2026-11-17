# Chapter 12 Lab — Scheduling Tasks

## RHCSA EX200 | Red Hat Enterprise Linux

### Objective
Learn how to schedule and manage recurring and one-time tasks using systemd timers, cron, anacron, and at.

## 1. Systemd Timers

Systemd timers schedule the execution of systemd services.

A `.timer` unit defines when a task runs, while a `.service` unit defines what executes.

**Commands practised:**

```bash
systemctl cat logrotate.timer
systemctl cat logrotate.service
systemctl list-timers --all
systemctl status logrotate.timer
systemctl cat sysstat-collect.timer
```

**Important settings:**
- `OnCalendar=` — calendar-based scheduling
- `OnBootSec=` — time elapsed since system boot
- `OnActiveSec=` — time elapsed since timer activation
- `Persistent=true` — catches up on missed calendar executions

## 2. Cron Jobs

Cron schedules recurring tasks using five time fields.

```text
minute hour day-of-month month day-of-week command
```

Example:

```cron
0 23 * * * /usr/bin/date
```

Runs every day at 11 p.m.

**Commands practised:**

```bash
systemctl status crond
crontab -e
crontab -l
```

## 3. System-Wide Cron Jobs

System-wide cron jobs can be configured in `/etc/cron.d/`.

Unlike a personal crontab, these entries specify the user who executes the command.

Example:

```cron
* * * * * root /usr/bin/date >> /var/log/cron-test.log
```

## 4. Cron Scheduling Directories

- `/etc/cron.hourly/`
- `/etc/cron.daily/`
- `/etc/cron.weekly/`
- `/etc/cron.monthly/`

These directories contain scripts intended to run at the specified frequency.

Executable scripts can be prepared with:

```bash
chmod +x script.sh
```

## 5. Anacron

Anacron manages periodic tasks that may have been missed while a system was unavailable.

Configuration file:

```bash
/etc/anacrontab
```

## 6. Cron Security

Cron access can be controlled using:

```text
/etc/cron.allow
/etc/cron.deny
```

If `cron.allow` exists, it determines which users are permitted to use crontab.

## 7. One-Time Scheduling with at

The `at` command schedules a task to execute once.

```bash
at now + 2 minutes
atq
atrm JOB_NUMBER
```

The `atd` service manages scheduled at jobs.

## 8. Creating a Custom Systemd Timer

Practice unit files:

```text
/etc/systemd/system/mybackup.service
/etc/systemd/system/mybackup.timer
```

**Service configuration:**

```ini
[Unit]
Description=My first scheduled systemd service

[Service]
Type=oneshot
ExecStart=/usr/bin/date
StandardOutput=append:/var/log/mybackup.log
```

**Timer configuration:**

```ini
[Unit]
Description=Run mybackup service after boot

[Timer]
OnBootSec=2min
Unit=mybackup.service

[Install]
WantedBy=timers.target
```

**Management commands:**

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now mybackup.timer
systemctl status mybackup.timer
systemctl status mybackup.service
```

## Key Learning Outcomes

- Understand the differences between cron, anacron, at, and systemd timers.
- Configure recurring scheduled tasks.
- Manage one-time jobs.
- Control crontab access.
- Create and manage custom systemd timer units.
- Verify scheduled jobs and troubleshoot scheduling issues.

## RHCSA Preparation

Chapter 12 focuses on scheduling administrative tasks and verifying that they execute as required.

**Note:** This README documents the Chapter 12 learning exercises. The final end-of-chapter practical assessment is separate.
