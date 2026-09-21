Day 07 – Linux File System Hierarchy & Scenario-Based Practice

Objective

Today I learned about the Linux File System Hierarchy and practiced troubleshooting common DevOps scenarios.

The main goal was to understand:

* Where Linux stores files and configurations
* Where logs are located
* Where user files are stored
* How to troubleshoot services
* How to identify high CPU usage
* How to find service logs
* How to troubleshoot file permission issues

⸻

Part 1: Linux File System Hierarchy

Linux organizes files and directories under a single root directory called /.

1. / — Root Directory

Purpose

/ is the starting point of the Linux file system. Almost everything in Linux exists somewhere under this directory.

Command

ls -l /

I would use this when…

I would use / when I need to understand the main directories available on a Linux system.

⸻

2. /home — User Home Directories

Purpose

/home contains the personal directories of normal users.

For example:

/home/faiz_malik

User-specific files, documents, downloads, and configurations are generally stored here.

Command

ls -l /home

I would use this when…

I would use /home when I need to access or manage a user’s personal files.

⸻

3. /root — Root User’s Home

Purpose

/root is the home directory of the root user.

It is different from /, which is the root of the entire file system.

Command

sudo ls -l /root

I would use this when…

I would use /root when troubleshooting or managing files belonging to the root administrator account.

⸻

4. /etc — Configuration Files

Purpose

/etc contains system and application configuration files.

Examples include:

/etc/hosts
/etc/hostname
/etc/passwd
/etc/ssh/

Command

ls -l /etc

I would use this when…

I would use /etc when I need to check or modify system and service configuration.

Example

cat /etc/hostname

This displays the hostname of the Linux machine.

⸻

5. /var/log — Log Files

Purpose

/var/log contains system and application logs.

Examples include:

/var/log/syslog
/var/log/auth.log

Logs are extremely important when troubleshooting Linux servers.

Command

ls -l /var/log

Find large log files

du -sh /var/log/* 2>/dev/null | sort -h | tail -5

I would use this when…

I would use /var/log when investigating errors, service failures, authentication problems, or other system issues.

⸻

6. /tmp — Temporary Files

Purpose

/tmp is used for temporary files created by users and applications.

These files are generally not intended for permanent storage.

Command

ls -l /tmp

I would use this when…

I would use /tmp for temporary files, testing, scripts, and short-term troubleshooting data.

⸻

Additional Directories

7. /bin — Essential Commands

Purpose

/bin contains essential command binaries required for basic system operations.

Examples include commands such as:

ls
cp
mv
cat

On modern Ubuntu systems, /bin may be linked to /usr/bin.

Command

ls -l /bin

I would use this when…

I would use /bin when I need to understand where essential Linux commands are located.

⸻

8. /usr/bin — User Command Binaries

Purpose

/usr/bin contains many executable programs and commands used by normal users.

Command

ls -l /usr/bin | head

I would use this when…

I would use /usr/bin when locating installed command-line programs.

⸻

9. /opt — Optional/Third-Party Applications

Purpose

/opt is commonly used for optional or third-party software that is installed outside the standard package locations.

Command

ls -l /opt

I would use this when…

I would use /opt when checking software installed manually or by third-party applications.

⸻

Important Directories Summary

Directory	Purpose	Common DevOps Use
/	Root of the file system	Understand system structure
/home	Normal user files	User data and files
/root	Root user’s home	Administrator files
/etc	Configuration files	Service/system configuration
/var/log	Log files	Troubleshooting
/tmp	Temporary files	Testing and temporary data
/bin	Essential commands	Basic system commands
/usr/bin	User commands	Installed CLI tools
/opt	Optional software	Third-party applications

⸻

Hands-on File System Checks

Find Large Log Files

du -sh /var/log/* 2>/dev/null | sort -h | tail -5

Explanation

* du -sh → shows directory/file size in human-readable format
* /var/log/* → checks items inside /var/log
* 2>/dev/null → hides permission/error messages
* sort -h → sorts sizes correctly
* tail -5 → shows the last 5 entries

This helps identify large log files that may consume disk space.

⸻

Check System Hostname

cat /etc/hostname

This shows the hostname configured for the Linux system.

⸻

Check Home Directory

ls -la ~

~ represents the current user’s home directory.

-a shows hidden files and -l provides detailed information.

⸻

Part 2: Scenario-Based Practice

Scenario 1: Service Not Starting

Problem

A web application service called myapp failed to start after a server reboot.

Step 1: Check Service Status

systemctl status myapp

Why?

This tells us whether the service is running, stopped, or failed.

It also shows the main process and recent service messages.

⸻

Step 2: Check Service Logs

journalctl -u myapp -n 50

Why?

The logs can show the actual reason for the failure, such as:

* Configuration error
* Missing file
* Permission problem
* Port already in use
* Application crash

⸻

Step 3: Check Whether the Service Is Enabled

systemctl is-enabled myapp

Why?

This checks whether the service is configured to start automatically during boot.

⸻

Step 4: Try Restarting After Understanding the Error

sudo systemctl restart myapp

Then verify:

systemctl status myapp

Why?

After identifying and fixing the cause, restarting the service confirms whether the problem has been resolved.

Troubleshooting Flow

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

⸻

Scenario 2: High CPU Usage

Problem

The application server is slow and we need to identify which process is consuming high CPU.

Step 1: Check Live CPU Usage

top

Why?

top shows running processes and their CPU/memory usage in real time.

Look at the %CPU column.

Press:

q

to exit top.

⸻

Step 2: Sort Processes by CPU Usage

ps aux --sort=-%cpu | head -10

Why?

This displays processes sorted by CPU usage, with the highest CPU-consuming processes at the top.

⸻

Step 3: Identify the PID

Example:

PID
1234

The PID is the Process ID of the process.

Once the problematic process is identified, investigate it further.

⸻

Step 4: Inspect the Process

ps -p <PID> -o pid,ppid,pcpu,pmem,comm

Replace <PID> with the actual process ID.

Why?

This provides focused information about the selected process instead of looking at the entire process list.

⸻

Scenario 3: Finding Service Logs

Problem

A developer asks:

Where are the logs for the docker service?

If the service is managed by systemd, its logs are normally available through journald.

Step 1: Check Service Status

systemctl status docker

Why?

This confirms whether the Docker service exists and shows its current status.

⸻

Step 2: View Recent Logs

journalctl -u docker -n 50

Why?

-u docker filters logs for the Docker service.

-n 50 shows the latest 50 log lines.

⸻

Step 3: Follow Logs in Real Time

journalctl -u docker -f

Why?

-f follows new log entries as they appear.

This is useful while reproducing an issue.

Press:

Ctrl + C

to stop following the logs.

⸻

Scenario 4: File Permission Issue

Problem

A script is not executing:

./backup.sh

Error:

Permission denied

Step 1: Check Current Permissions

ls -l /home/user/backup.sh

Example:

-rw-r--r-- 1 user user 250 Sep 21 10:00 backup.sh

Why?

The permission string does not contain x.

x means execute permission.

⸻

Step 2: Add Execute Permission

chmod +x /home/user/backup.sh

Why?

chmod +x adds execute permission to the file.

⸻

Step 3: Verify Permissions

ls -l /home/user/backup.sh

Example:

-rwxr-xr-x 1 user user 250 Sep 21 10:00 backup.sh

Now the file has execute permission.

⸻

Step 4: Run the Script

./backup.sh

Why?

Now that execute permission has been added, we can run the script.

⸻

Troubleshooting Mindset

The main lesson from these scenarios is:

Don't guess → Check → Observe → Find the cause → Fix → Verify

For example, when a service fails:

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

This approach is more reliable than randomly restarting services or changing configurations.

⸻

Useful Commands

Command	Purpose
ls -l	Detailed file listing
ls -la	Detailed listing including hidden files
cat	Read file contents
du -sh	Check file/directory size
sort -h	Sort human-readable sizes
systemctl status	Check service status
systemctl is-enabled	Check boot-time enablement
journalctl -u	View service logs
top	Monitor processes
ps aux	List processes
chmod +x	Add execute permission

⸻

Why This Matters for DevOps

Understanding the Linux file system helps me quickly locate:

* Configuration files
* Application logs
* User files
* Temporary files
* Installed software
* System commands

Scenario-based troubleshooting also builds the habit of investigating problems systematically instead of guessing.

These skills are useful for:

* Production troubleshooting
* Server administration
* Deployment debugging
* DevOps interviews
* On-call incident handling

⸻

Day 07 Takeaway

Today I learned the purpose of important Linux directories such as /etc, /var/log, /home, /tmp, /usr/bin, and /opt.

I also practiced a basic troubleshooting approach for:

1. Service startup failures
2. High CPU usage
3. Finding systemd service logs
4. File permission problems

Key Lesson

Understand the system first, check the evidence, then take action.

Day 07 completed ✅
