# Day 07 – Linux File System Hierarchy & Scenario-Based Practice

## Objective

Today, I learned about the Linux file system hierarchy and practiced troubleshooting common DevOps issues. The main goal was to understand where Linux stores files, configurations, logs, and user data, and how to diagnose common system problems efficiently.

The key learning areas were:

- Where Linux stores files and configurations
- Where logs are located
- Where user files are stored
- How to troubleshoot services
- How to identify high CPU usage
- How to locate service logs
- How to fix file permission problems

---

## Part 1: Linux File System Hierarchy

Linux organizes all files and directories under a single root directory called `/`.

### 1. `/` — Root Directory

The `/` directory is the top-level root of the Linux filesystem. Almost everything in the system exists somewhere under this directory.

Command:

```bash
ls -l /
```

This command helps me view the main directories present on the system and understand the overall filesystem layout.

I would use `/` when I need to explore the basic structure of a Linux machine and locate important system directories.

---

### 2. `/home` — User Home Directories

The `/home` directory contains personal directories for regular users. Each user typically has their own folder inside this directory.

Example:

```bash
/home/faiz_malik
```

This directory usually stores personal files such as documents, downloads, code, and configuration files.

Command:

```bash
ls -l /home
```

I would use `/home` when I need to access or manage a user’s personal files.

---

### 3. `/root` — Root User’s Home

The `/root` directory is the home directory for the root user, which is the administrator account in Linux.

It is different from `/`, which is the root of the entire filesystem.

Command:

```bash
sudo ls -l /root
```

I would use `/root` when troubleshooting or managing files and settings related to the root administrator account.

---

### 4. `/etc` — Configuration Files

The `/etc` directory contains system and application configuration files. This is one of the most important directories for system administration.

Examples include:

- `/etc/hosts`
- `/etc/hostname`
- `/etc/passwd`
- `/etc/ssh/`

Command:

```bash
ls -l /etc
```

I would use `/etc` when I need to check or modify configuration files for services or the operating system.

Example:

```bash
cat /etc/hostname
```

This command displays the hostname configured for the Linux machine.

---

### 5. `/var/log` — Log Files

The `/var/log` directory stores system and application logs. These logs are essential when troubleshooting problems on Linux servers.

Examples include:

- `/var/log/syslog`
- `/var/log/auth.log`

Logs are extremely useful when investigating service failures, authentication problems, or unexpected system behavior.

Command:

```bash
ls -l /var/log
```

To find the largest log files:

```bash
du -sh /var/log/* 2>/dev/null | sort -h | tail -5
```

I would use `/var/log` when investigating errors, service issues, failed authentication, or system events.

---

### 6. `/tmp` — Temporary Files

The `/tmp` directory is used for temporary files created by users and applications. These files are generally not meant for permanent storage.

Command:

```bash
ls -l /tmp
```

I would use `/tmp` for temporary files, testing, scripts, and short-term troubleshooting data.

---

## Additional Important Directories

### 7. `/bin` — Essential Commands

The `/bin` directory contains essential command binaries required for basic system operations.

Examples of commands in this directory include:

- `ls`
- `cp`
- `mv`
- `cat`

On modern Ubuntu systems, `/bin` may point to `/usr/bin` through symbolic links.

Command:

```bash
ls -l /bin
```

I would use `/bin` when I need to understand where essential Linux commands are located.

---

### 8. `/usr/bin` — User Command Binaries

The `/usr/bin` directory contains many executable programs and command-line tools used by regular users.

Command:

```bash
ls -l /usr/bin | head
```

I would use `/usr/bin` when I need to locate installed command-line tools and software.

---

### 9. `/opt` — Optional or Third-Party Applications

The `/opt` directory is typically used for optional or third-party software installed outside the standard package directories.

Command:

```bash
ls -l /opt
```

I would use `/opt` when checking software installed manually or by third-party vendors.

---

## Important Directories Summary

| Directory | Purpose | Common DevOps Use |
|----------|---------|------------------|
| `/` | Root of the filesystem | Understand system structure |
| `/home` | User home directories | User data and files |
| `/root` | Root user’s home directory | Administrator files |
| `/etc` | Configuration files | Service and system configuration |
| `/var/log` | Log files | Troubleshooting |
| `/tmp` | Temporary files | Testing and temporary data |
| `/bin` | Essential system commands | Basic system operations |
| `/usr/bin` | User commands | Installed CLI tools |
| `/opt` | Optional software | Third-party apps |

---

## Hands-on File System Checks

### Find Large Log Files

```bash
du -sh /var/log/* 2>/dev/null | sort -h | tail -5
```

This command helps identify large files within `/var/log`. It is useful when disk space is running low and I need to determine whether logs are consuming too much storage.

Explanation:

- `du -sh` → shows directory or file size in human-readable format
- `/var/log/*` → checks contents of `/var/log`
- `2>/dev/null` → hides permission or error messages
- `sort -h` → sorts sizes correctly
- `tail -5` → displays the last 5 entries

---

### Check the System Hostname

```bash
cat /etc/hostname
```

This command shows the hostname configured for the Linux system.

---

### Check the Current User’s Home Directory

```bash
ls -la ~
```

The `~` symbol represents the current user’s home directory.

The `-a` option shows hidden files, and `-l` provides detailed file information.

---

## Part 2: Scenario-Based Practice

### Scenario 1: Service Not Starting

A web application service called `myapp` failed to start after a server reboot.

#### Step 1: Check Service Status

```bash
systemctl status myapp
```

This tells me whether the service is running, stopped, or failed. It also shows important process and runtime information.

#### Step 2: Check Service Logs

```bash
journalctl -u myapp -n 50
```

Logs often reveal the actual reason the service failed, such as:

- Configuration error
- Missing file
- Permission problem
- Port already in use
- Application crash

#### Step 3: Check Whether the Service Is Enabled

```bash
systemctl is-enabled myapp
```

This determines whether the service is configured to start automatically during boot.

#### Step 4: Restart the Service After Fixing the Cause

```bash
sudo systemctl restart myapp
```

Then verify:

```bash
systemctl status myapp
```

This confirms whether the fix worked.

#### Troubleshooting Flow

```text
Service failed
   ↓
Check status
   ↓
Check logs
   ↓
Find the cause
   ↓
Fix the cause
   ↓
Restart service
   ↓
Verify status
```

This approach is more reliable than guessing or restarting services randomly.

---

### Scenario 2: High CPU Usage

The application server is slow, and we need to identify which process is using the most CPU.

#### Step 1: Check Live CPU Usage

```bash
top
```

The `top` command shows running processes and their CPU and memory usage in real time. I can look at the `%CPU` column to identify the culprit.

To exit `top`, press:

```bash
q
```

#### Step 2: Sort Processes by CPU Usage

```bash
ps aux --sort=-%cpu | head -10
```

This lists processes sorted by CPU usage, with the highest consumer at the top.

#### Step 3: Identify the PID

Example:

```bash
PID
1234
```

The PID is the Process ID of the process. Once the problematic process is identified, I can investigate it further.

#### Step 4: Inspect the Process

```bash
ps -p <PID> -o pid,ppid,pcpu,pmem,comm
```

Replace `<PID>` with the actual process ID.

This gives a focused view of the selected process instead of showing the entire process list.

---

### Scenario 3: Finding Service Logs

A developer asks: “Where are the logs for the Docker service?”

If the service is managed by `systemd`, its logs are usually available through journald.

#### Step 1: Check Service Status

```bash
systemctl status docker
```

This confirms whether the Docker service exists and shows its current state.

#### Step 2: View Recent Logs

```bash
journalctl -u docker -n 50
```

The `-u docker` option filters logs for the Docker service, and `-n 50` shows the latest 50 lines.

#### Step 3: Follow Logs in Real Time

```bash
journalctl -u docker -f
```

The `-f` option follows new log entries as they are written. This is useful while reproducing an issue.

To stop following the logs, press:

```bash
Ctrl + C
```

---

### Scenario 4: File Permission Issue

A script is not executing:

```bash
./backup.sh
```

It returns:

```bash
Permission denied
```

#### Step 1: Check Current Permissions

```bash
ls -l /home/user/backup.sh
```

Example output:

```bash
-rw-r--r-- 1 user user 250 Sep 21 10:00 backup.sh
```

This means the file does not have execute permission. The `x` flag is missing.

#### Step 2: Add Execute Permission

```bash
chmod +x /home/user/backup.sh
```

This adds execute permission to the file.

#### Step 3: Verify the Permission Change

```bash
ls -l /home/user/backup.sh
```

Example:

```bash
-rwxr-xr-x 1 user user 250 Sep 21 10:00 backup.sh
```

Now the script has execute permission.

#### Step 4: Run the Script

```bash
./backup.sh
```

Once the permission issue is fixed, the script can run successfully.

---

## Troubleshooting Mindset

The biggest lesson from these scenarios is:

> Don’t guess. Check, observe, find the cause, fix it, and verify the result.

For example, when a service fails:

```text
systemctl status
      ↓
journalctl
      ↓
identify error
      ↓
fix problem
      ↓
restart
      ↓
verify
```

This approach is much more reliable than randomly restarting services or changing configurations without evidence.

---

## Useful Commands

| Command | Purpose |
|---------|---------|
| `ls -l` | Detailed file listing |
| `ls -la` | Detailed listing including hidden files |
| `cat` | Read file contents |
| `du -sh` | Check file or directory size |
| `sort -h` | Sort human-readable sizes |
| `systemctl status` | Check service status |
| `systemctl is-enabled` | Check if a service starts automatically |
| `journalctl -u` | View service logs |
| `top` | Monitor system processes |
| `ps aux` | List running processes |
| `chmod +x` | Add execute permission |

---

## Why This Matters for DevOps

Understanding the Linux file system helps me quickly locate:

- Configuration files
- Application logs
- User files
- Temporary files
- Installed software
- System commands

Scenario-based troubleshooting also builds the habit of investigating problems systematically instead of guessing.

These skills are useful for:

- Production troubleshooting
- Server administration
- Deployment debugging
- DevOps interviews
- On-call incident response

---

## Day 07 Takeaway

Today I learned the purpose of important Linux directories such as `/etc`, `/var/log`, `/home`, `/tmp`, `/usr/bin`, and `/opt`.

I also practiced a systematic troubleshooting approach for:

1. Service startup failures
2. High CPU usage
3. Finding systemd service logs
4. File permission problems

### Key Lesson

Understand the system first, check the evidence, and then take action.

Day 07 completed ✅
```
