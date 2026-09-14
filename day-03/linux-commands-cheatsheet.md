# Linux Commands Cheatsheet

## 1. File System Commands

| Command                  | Usage                                                       |
| ------------------------ | ----------------------------------------------------------- |
| `pwd`                    | Shows the current working directory.                        |
| `ls`                     | Lists files and folders in the current directory.           |
| `ls -la`                 | Shows all files, including hidden files, with details.      |
| `cd folder`              | Moves into a specific folder.                               |
| `cd ..`                  | Moves one directory back.                                   |
| `mkdir folder`           | Creates a new directory.                                    |
| `touch file.md`          | Creates a new empty file.                                   |
| `cp file.txt backup.txt` | Copies a file.                                              |
| `mv file.txt folder/`    | Moves a file to another directory.                          |
| `rm file.txt`            | Deletes a file.                                             |
| `rmdir folder`           | Deletes an empty directory.                                 |
| `cat file.txt`           | Displays the contents of a file.                            |
| `less file.txt`          | Opens a file and allows you to scroll through its contents. |
| `head file.txt`          | Shows the first lines of a file.                            |
| `tail file.txt`          | Shows the last lines of a file.                             |

## 2. Process Management

| Command       | Usage                                                                 |
| ------------- | --------------------------------------------------------------------- |
| `ps`          | Shows currently running processes.                                    |
| `ps aux`      | Shows detailed information about running processes.                   |
| `top`         | Displays running processes and system resource usage in real time.    |
| `htop`        | Interactive process viewer; usually needs to be installed separately. |
| `kill PID`    | Stops a process using its process ID.                                 |
| `kill -9 PID` | Forcefully terminates a process.                                      |
| `jobs`        | Shows jobs running in the current terminal session.                   |
| `bg`          | Runs a stopped job in the background.                                 |
| `fg`          | Brings a background job to the foreground.                            |

## 3. Networking Commands

| Command                    | Usage                                                |
| -------------------------- | ---------------------------------------------------- |
| `ip addr`                  | Shows network interfaces and IP addresses.           |
| `ping google.com`          | Checks whether a host is reachable over the network. |
| `curl https://example.com` | Sends a request to a URL and displays the response.  |
| `dig google.com`           | Checks DNS information for a domain.                 |
| `ss -tuln`                 | Shows listening TCP and UDP network ports.           |

## 4. Disk and System Information

| Command          | Usage                                                            |
| ---------------- | ---------------------------------------------------------------- |
| `df -h`          | Shows available and used disk space in a readable format.        |
| `du -sh folder/` | Shows the total size of a directory.                             |
| `free -h`        | Shows RAM and swap memory usage.                                 |
| `uptime`         | Shows how long the system has been running and its load average. |
| `uname -a`       | Displays Linux kernel and system information.                    |

## 5. Useful Troubleshooting Commands

| Command         | Usage                                                    |
| --------------- | -------------------------------------------------------- |
| `whoami`        | Shows the currently logged-in user.                      |
| `id`            | Shows the current user's UID, GID and group information. |
| `history`       | Shows previously executed commands.                      |
| `which command` | Shows the location of an executable command.             |
| `man command`   | Opens the manual/help page for a command.                |

## Quick Examples

### Check where I am

```bash
pwd
```

### Create a folder and file

```bash
mkdir devops
cd devops
touch notes.md
```

### Check running processes

```bash
ps aux
```

### Check my IP address

```bash
ip addr
```

### Check network connectivity

```bash
ping google.com
```

### Check a website

```bash
curl https://example.com
```

### Check disk space

```bash
df -h
```

### Check memory

```bash
free -h
```

## My Key Takeaway

Linux commands are an important part of DevOps because most servers and cloud environments are managed from the command line. I will focus on understanding the commands and practicing them instead of only memorizing them.

