# Day 02 – Linux Architecture, Processes and systemd

## 1. How Linux Works

Linux is an operating system that helps applications communicate with the computer's hardware.

The main parts are:

**Kernel:**
The kernel is the core part of Linux. It manages CPU, memory, processes, files, devices, and networking. Applications don't directly control the hardware. They communicate with the kernel when they need something.

**User Space:**
This is where normal applications and commands run, such as `bash`, `nginx`, `node`, `python`, `ls`, and many others.

**systemd:**
systemd is the main system and service manager on modern Ubuntu systems. It starts important services when the system boots and helps us start, stop, restart, and monitor services.

A simple way to understand it is:

```text
Application
     ↓
User Space
     ↓
Linux Kernel
     ↓
Hardware
```

## 2. Processes

A process is simply a program that is currently running.

For example, when I run:

```bash
sleep 100
```

Linux creates a process for it.

Every process has a **PID (Process ID)**, which is a unique number used to identify that process.

Processes can also have a **PPID (Parent Process ID)**. This tells us which process created or started another process.

### Common Process States

**Running (R):** The process is currently running or ready to run.

**Sleeping (S):** The process is waiting for something, such as input or a response.

**Uninterruptible Sleep (D):** The process is usually waiting for I/O, such as disk activity.

**Zombie (Z):** The process has finished, but its parent process has not yet collected its result.

I can check processes using:

```bash
ps aux
```

and:

```bash
ps -eo pid,ppid,state,cmd
```

## 3. systemd

systemd is responsible for managing many services in Linux.

For example, if I install Nginx, I can use systemd to manage it:

```bash
sudo systemctl status nginx
```

I can start it with:

```bash
sudo systemctl start nginx
```

Restart it with:

```bash
sudo systemctl restart nginx
```

And check its logs with:

```bash
journalctl -u nginx
```

This is very useful in DevOps because when a service stops working, I can check its status and logs instead of guessing what went wrong.

## 4. Five Useful Linux Commands

```bash
ps aux
```

Shows running processes.

```bash
top
```

Shows CPU, memory, and running processes in real time.

```bash
systemctl status <service>
```

Checks whether a service is running.

```bash
journalctl -u <service>
```

Shows logs related to a service.

```bash
ps -eo pid,ppid,state,cmd
```

Shows process ID, parent process ID, process state, and command.

## 5. Why This Matters in DevOps

Linux is used heavily on servers and cloud platforms.

As a DevOps engineer, I need to understand processes and services because applications can crash, use too much CPU or memory, or stop responding.

Knowing commands like `ps`, `top`, `systemctl`, and `journalctl` helps me find the problem and troubleshoot it faster.

**My main takeaway:**
Linux is not just about running commands. I need to understand what is happening behind those commands. The kernel manages the system, processes run applications, and systemd manages important services.

