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