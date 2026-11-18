# Day 12 – Breather & Revision (Days 01–11)

## Goal
Revise Linux fundamentals from Days 01–11 and strengthen practical command-line skills.

## 1. Mindset & Learning Plan
- Continue building Linux and DevOps fundamentals.
- Focus on practical Linux troubleshooting.
- Improve knowledge of Docker, AWS, CI/CD, and cloud technologies.
- Practise commands regularly instead of only memorising them.

## 2. Processes & Services

### Process Management

| Command | Purpose |
|---|---|
| `ps aux` | Displays running processes |
| `ps aux \| head -10` | Displays the first 10 process entries |
| `top` | Monitors processes in real time |
| `pgrep nginx` | Finds a process by name |
| `kill PID` | Sends a termination signal to a process |

### Service Management

| Command | Purpose |
|---|---|
| `systemctl status cron` | Checks cron service status |
| `systemctl start cron` | Starts cron service |
| `systemctl stop cron` | Stops cron service |
| `systemctl restart cron` | Restarts cron service |
| `journalctl -u cron` | Displays cron service logs |
| `journalctl -u cron -n 20` | Displays the latest 20 log entries |

## 3. File Skills

| Command | Purpose |
|---|---|
| `touch file.txt` | Creates an empty file |
| `mkdir folder` | Creates a directory |
| `mkdir -p a/b` | Creates nested directories |
| `cp file.txt backup.txt` | Copies a file |
| `mv old.txt new.txt` | Renames or moves a file |
| `cat file.txt` | Displays file content |
| `echo "Hello" >> file.txt` | Appends text to a file |
| `chmod 755 file.txt` | Changes file permissions |
| `chown user file.txt` | Changes file owner |
| `chgrp group file.txt` | Changes group ownership |
| `ls -l` | Displays permissions and ownership |

### Removing Files and Directories
```bash
rm file.txt
rmdir folder
rm -r folder
```

- `rmdir` removes empty directories only.
- `rm -r` removes directories and their contents.
- `rm -rf` forcefully removes files and directories. Use it carefully.

## 4. Five Important Commands for Troubleshooting

| Command | Purpose |
|---|---|
| `top` | Checks CPU and memory usage by processes |
| `df -h` | Checks disk space |
| `free -h` | Checks memory usage |
| `systemctl status nginx` | Checks Nginx service status |
| `journalctl -u nginx -n 50` | Checks the latest 50 Nginx log entries |

## 5. User and Group Management

| Command | Purpose |
|---|---|
| `whoami` | Shows current username |
| `id` | Shows user and group IDs |
| `useradd` | Creates a user |
| `passwd` | Changes a password |
| `groupadd` | Creates a group |
| `usermod -aG` | Adds a user to a supplementary group |
| `groups` | Displays group membership |
| `chown` | Changes file ownership |
| `chgrp` | Changes group ownership |

### Example
```bash
sudo useradd -m tokyo
sudo groupadd developers
sudo usermod -aG developers tokyo
id tokyo
```

## 6. Useful System and Networking Commands

### System Information
```bash
pwd
whoami
hostname
uname -a
uptime
date
```

### Disk and Memory
```bash
df -h
du -sh /var/log
free -h
lsblk
```

### Networking
```bash
ip addr
ip route
ping -c 4 google.com
curl -I https://google.com
ss -tulnp
```

### Searching and Logs
```bash
grep "error" app.log
grep -i "failed" app.log
tail -f /var/log/syslog
history
```

## 7. Mini Self-Check

1. **Which three commands save me the most time right now?**
   - `ps aux` – To check running processes.
   - `systemctl status` – To check service health.
   - `df -h` – To check available disk space.
2. **How do I check if a service is healthy?**
   - `systemctl status nginx`
   - `journalctl -u nginx -n 50`
   - `systemctl is-active nginx`
3. **How do I safely change ownership and permissions?**
   - First check existing ownership using `ls -l`.
   - Then use `sudo chown user:group filename`.
   - Use `chmod` to grant only the required permissions.
4. **What will I focus on improving in the next three days?**
   - Linux command practice.
   - File permissions and ownership.
   - Process and service troubleshooting.
   - Docker fundamentals.

## 8. What I Learned
- How to inspect running processes using `ps` and `top`.
- How to check and manage services using `systemctl`.
- How to investigate service logs using `journalctl`.
- How to create, copy, move, and remove files and directories.
- How to manage file permissions and ownership.
- How to manage Linux users and groups.
- How to check disk space, memory, and network information.
- The difference between `rmdir`, `rm -r`, and `rm -rf`.

---
