# Day 05 – Linux Troubleshooting Runbook

## 🎯 Objective

The goal of this troubleshooting drill was to inspect the health of a running Linux service by checking system resources, networking, service status, and logs.

For this drill, I selected **`cron`** as the target service.

---

## 🛠️ Environment

| Item            | Details          |
| --------------- | ---------------- |
| OS              | Ubuntu 26.04 LTS |
| Environment     | WSL2             |
| Kernel          | Linux 6.18.33.2  |
| Target Service  | `cron`           |
| Service Manager | `systemd`        |
| Shell           | Bash             |

---

# 1. Environment Basics

## 1.1 System Information

### Command

```bash
uname -a
```

### Output

```text
Linux faizmalik 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86_64 GNU/Linux
```

### Observation

The system is running **Linux on WSL2** with an `x86_64` architecture.

---

## 1.2 Operating System Information

### Command

```bash
cat /etc/os-release
```

### Output

```text
PRETTY_NAME="Ubuntu 26.04 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
VERSION="26.04 (Resolute Raccoon)"
VERSION_CODENAME=resolute
ID=ubuntu
```

### Observation

The system is running **Ubuntu 26.04 LTS**.

---

# 2. Filesystem Sanity Check

## 2.1 Create a Temporary Directory

### Command

```bash
mkdir /tmp/runbook-demo
```

### Observation

The temporary directory was created successfully.

---

## 2.2 Copy and Verify a File

### Command

```bash
cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo
```

### Output

```text
total 4
-rw-r--r-- 1 faiz_malik faiz_malik 415 Sep 17 17:32 hosts-copy
```

### Observation

The file was copied successfully, confirming that basic filesystem operations are working correctly.

---

# 3. CPU and Memory Snapshot

## 3.1 CPU and Process Check

### Command

```bash
top
```

### Output

```text
load average: 0.15, 0.74, 0.46

Tasks: 25 total, 1 running, 24 sleeping, 0 stopped, 0 zombie

%Cpu(s): 0.0 us, 0.2 sy, 0.0 ni, 99.8 id

MiB Mem : 1625.6 total, 662.3 free, 397.2 used, 640.0 buff/cache
MiB Swap: 1024.0 total, 1024.0 free, 0.0 used
```

### Observation

CPU usage was very low, with **99.8% CPU idle**. There were no zombie processes, and the system load was relatively low.

---

## 3.2 Memory Check

### Command

```bash
free -h
```

### Output

```text
               total        used        free      shared  buff/cache   available
Mem:           1.6Gi       398Mi       661Mi       3.4Mi       640Mi       1.2Gi
Swap:          1.0Gi          0B       1.0Gi
```

### Observation

The system had approximately **1.2 GiB available memory**, and swap usage was `0B`. No significant memory pressure was observed.

---

# 4. Disk and I/O Snapshot

## 4.1 Check Log Directory Size

### Command

```bash
sudo du -sh /var/log
```

### Output

```text
106M    /var/log
```

### Observation

The `/var/log` directory was using approximately **106 MB** of disk space.

---

> **Note:** `df -h` was not captured during this run. It should be run during a future troubleshooting drill to verify overall filesystem capacity.

---

# 5. Network Snapshot

## 5.1 Check Listening Ports

### Command

```bash
ss -tulpn
```

### Observation

The system had DNS-related listeners on port `53` and NTP-related listeners on port `323`. No unexpected application ports were observed in the captured output.

---

## 5.2 Test Network Connectivity

### Command

```bash
ping -c 4 8.8.8.8
```

### Output

```text
4 packets transmitted, 4 received, 0% packet loss

rtt min/avg/max/mdev =
72.172/164.028/284.130/76.645 ms
```

### Observation

Network connectivity was successful with **0% packet loss**.

The average latency was approximately **164 ms**, with some variation between packets.

---

# 6. Target Service – Cron

## 6.1 Check Cron Service Status

### Command

```bash
systemctl status cron
```

### Output

```text
● cron.service - Regular background program processing daemon
     Loaded: loaded (/usr/lib/systemd/system/cron.service; enabled)
     Active: active (running)
     Main PID: 209 (cron)
     Tasks: 1
     Memory: 432K (peak: 1.7M)
     CPU: 101ms
```

### Observation

The `cron` service is **active and running**.

The service is using very little memory and CPU. Its main process ID is **209**.

---

## 6.2 Check Cron Process Resources

### Command

```bash
ps -o pid,pcpu,pmem,comm -p $(pgrep cron)
```

### Output

```text
    PID %CPU %MEM COMMAND
    209  0.0  0.1 cron
```

### Observation

The cron process is using **0.0% CPU** and only **0.1% memory**, indicating no unusual resource consumption.

---

# 7. Logs Reviewed

## 7.1 Cron Journal Logs

### Command

```bash
journalctl -u cron -n 50
```

### Important Findings

The logs show that cron starts successfully and executes scheduled jobs such as:

```text
(root) CMD (cd / && run-parts --report /etc/cron.hourly)
```

The logs also contain this warning during startup:

```text
cron.service: Referenced but unset environment variable evaluates to an empty string: EXTRA_OPTS
```

### Observation

The `EXTRA_OPTS` message is a **warning**, not a service failure. The service continues running successfully after startup.

The logs also show previous cron service stops and starts associated with system shutdown/boot activity.

---

## 7.2 System Logs

### Command

```bash
tail -n 50 /var/log/syslog
```

### Important Findings

Recent system logs contained messages from:

* `ubuntu-insights`
* `wsl-pro-service`
* `chronyd`
* `systemd`

There were also warnings related to the Ubuntu Pro Windows Agent:

```text
could not connect to Windows Agent
```

and an Ubuntu Insights message:

```text
ERROR failed to collect insights:
report already exists for this period
```

### Observation

These messages are not directly related to the `cron` service and did not indicate a cron failure.

---

# 8. Quick Health Snapshot

| Area            | Result                        | Status |
| --------------- | ----------------------------- | ------ |
| OS              | Ubuntu 26.04 LTS on WSL2      | ✅      |
| Filesystem      | File creation/copy successful | ✅      |
| CPU             | 99.8% idle                    | ✅      |
| Memory          | 1.2 GiB available             | ✅      |
| Swap            | 0B used                       | ✅      |
| `/var/log`      | 106 MB                        | ✅      |
| Network         | 0% packet loss                | ✅      |
| Listening Ports | DNS/NTP listeners observed    | ✅      |
| Cron Service    | Active and running            | ✅      |
| Cron CPU        | 0.0%                          | ✅      |
| Cron Memory     | 0.1%                          | ✅      |
| Cron Logs       | Scheduled jobs working        | ✅      |
| Cron Warning    | `EXTRA_OPTS` unset            | ⚠️     |

---

# 9. Quick Findings

The troubleshooting drill showed that the system was generally healthy.

The most important findings were:

1. CPU usage was very low, with **99.8% idle CPU**.
2. Approximately **1.2 GiB memory was available** and no swap was being used.
3. `/var/log` was using **106 MB**.
4. Network connectivity to `8.8.8.8` was successful with **0% packet loss**.
5. The `cron` service was **active and running** with PID `209`.
6. The cron process was using only **0.0% CPU and 0.1% memory**.
7. Cron logs showed scheduled jobs running normally.
8. A startup warning was found because the `EXTRA_OPTS` environment variable was unset.

### Overall Status

```text
System: Healthy
Cron Service: Running
Resource Usage: Normal
Network: Connected
Logs: Reviewed
Warning: EXTRA_OPTS environment variable is unset
```

---

# 10. If This Worsens 🚨

If the cron service starts failing or consuming excessive resources, I would follow these steps.

### 1. Collect service and log information

```bash
systemctl status cron
journalctl -u cron -n 100
```

Look for repeated failures, errors, or unexpected restarts.

### 2. Check system resources

```bash
top
free -h
df -h
ps aux | grep cron
```

This helps identify CPU, memory, disk, or process-related problems.

### 3. Restart only after collecting evidence

```bash
sudo systemctl restart cron
```

Then verify:

```bash
systemctl status cron
```

If the problem continues, investigate the specific cron job and collect deeper diagnostics such as:

```bash
strace -p <PID>
```

---

# 11. Troubleshooting Flow

```text
             Service Problem
                    │
                    ▼
          Check Service Status
          systemctl status cron
                    │
                    ▼
             Check Resources
          CPU → Memory → Disk
                    │
                    ▼
             Check Network
               ss / ping
                    │
                    ▼
              Check Logs
          journalctl -u cron
                    │
                    ▼
            Identify Root Cause
                    │
              ┌─────┴─────┐
              │           │
           Resolved    Still Failing
              │           │
              ▼           ▼
           Monitor     Debug /
                       Restart /
                       Escalate
```

---

# 12. Key Commands Learned

| Command               | Purpose                                      |
| --------------------- | -------------------------------------------- |
| `uname -a`            | Check kernel and system information          |
| `cat /etc/os-release` | Check OS information                         |
| `mkdir`               | Create a directory                           |
| `cp`                  | Copy files                                   |
| `top`                 | Monitor CPU and processes                    |
| `free -h`             | Check memory usage                           |
| `du -sh`              | Check directory size                         |
| `df -h`               | Check filesystem usage                       |
| `ss -tulpn`           | Check listening ports                        |
| `ping`                | Test network connectivity                    |
| `systemctl status`    | Check service status                         |
| `ps`                  | Inspect process resources                    |
| `journalctl`          | Inspect systemd service logs                 |
| `tail`                | View recent log entries                      |
| `strace`              | Perform deeper process-level troubleshooting |

---

## 💡 DevOps Takeaway

The main lesson from this drill is:

> **Collect evidence before taking action.**

Instead of immediately restarting a service, follow a systematic approach:

```text
CPU → Memory → Disk → Network → Service → Logs → Root Cause
```

This creates a repeatable troubleshooting workflow that can be used during real-world DevOps incidents.

---

## 📌 Day 05 Summary

> Today I practiced a Linux troubleshooting workflow by inspecting the `cron` service on Ubuntu WSL2. I checked system resources, filesystem operations, network connectivity, service status, process usage, and logs. I also identified an `EXTRA_OPTS` warning in the cron service logs and learned how to separate warnings from actual service failures.
