# Day 04 – Linux Practice: Processes and Services

## 1. Process Checks

### `ps`

```bash
ps
```

**What I learned:**
The `ps` command shows processes running in the current terminal/session.

**Output:**

```text
PID TTY          TIME CMD
314 pts/0    00:00:00 bash
572 pts/0    00:00:00 ps
```

The `bash` process is my current shell, and `ps` is the command I executed.

---

### `ps aux`

```bash
ps aux
```

**What I learned:**
`ps aux` shows detailed information about processes running on the system, including the user, PID, CPU usage, memory usage and command.

I observed important system processes such as:

```text
systemd
systemd-journald
systemd-resolved
cron
rsyslogd
chronyd
```

---

### `top`

```bash
top
```

**What I learned:**
`top` displays running processes and system resource usage in real time.

My system showed:

```text
Tasks: 27 total, 1 running, 26 sleeping
CPU: 99.8% idle
Memory: 1625.6 MiB total
```

This helped me understand the current CPU, memory and process activity of my Linux system.

---

## 2. Service Checks

### `systemctl status cron`

```bash
systemctl status cron
```

**What I learned:**
This command shows the current status of the cron service.

My result showed:

```text
Active: active (running)
Main PID: 151 (cron)
```

This means the cron service is currently running.

I also noticed a warning related to an unset environment variable:

```text
Referenced but unset environment variable evaluates to an empty string
```

However, the cron service itself is running successfully.

---

### `systemctl list-units --type=service --state=running`

```bash
systemctl list-units --type=service --state=running
```

**What I learned:**
This command lists the services that are currently running.

My system showed 13 running services, including:

```text
chrony.service
cron.service
dbus.service
rsyslog.service
systemd-journald.service
systemd-logind.service
systemd-resolved.service
systemd-udevd.service
```

---

## 3. Log Checks

### `journalctl -u cron -n 50`

```bash
journalctl -u cron -n 50
```

**What I learned:**
`journalctl` is used to view logs collected by systemd.

The `-u cron` option filters the logs for the cron service, while `-n 50` displays the latest 50 log entries.

I observed cron jobs running successfully, including hourly and daily jobs.

For example:

```text
(CRON) INFO (Running @reboot jobs)
(root) CMD (cd / && run-parts --report /etc/cron.hourly)
```

I also observed sessions being opened and closed for scheduled cron tasks.

---

## 4. Process Verification

### `pgrep cron`

```bash
pgrep cron
```

**Output:**

```text
151
```

**What I learned:**
`pgrep` searches for a running process by its name.

The result `151` is the PID of the running cron process.

I also confirmed this PID using `systemctl status cron`, where the main PID was:

```text
Main PID: 151
```

This helped me connect the process ID with the systemd service.

---

## 5. Mini Troubleshooting Flow

If a service is not working, I can follow these steps:

```text
Check service status
        ↓
Identify the service PID
        ↓
Check recent service logs
        ↓
Find warnings or errors
        ↓
Take corrective action
        ↓
Check the service status again
```

For example:

```bash
systemctl status cron
pgrep cron
journalctl -u cron -n 50
```

These commands help me determine whether the service is running and whether its logs contain useful information about problems.

## Key Takeaway

Today I practiced checking Linux processes, inspecting a systemd service and reading service logs.

The most useful thing I learned was how these commands work together. I can check the service with `systemctl`, find its process using `pgrep`, and investigate its activity using `journalctl`.

These are important troubleshooting skills for DevOps because many server problems can be investigated directly from the Linux command line.
