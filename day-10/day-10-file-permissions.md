# Day 10 – File Permissions & File Operations Challenge

## Overview

Today I practiced basic Linux file operations and learned how to manage file and directory permissions.

I worked with commands such as `touch`, `cat`, `vim`, `head`, `tail`, `ls -l`, and `chmod`.

I also created a shell script, made it executable, and tested how Linux handles restricted file permissions.

## Files Created

| File/Directory | Purpose |
|---|---|
| `devops.txt` | Empty text file |
| `notes.txt` | Stores learning notes |
| `script.sh` | Bash script that prints Hello DevOps |
| `project/` | Directory created with 755 permissions |

## Permission Changes

| File/Directory | Permission | Description |
|---|---|---|
| `script.sh` | 755 | Owner can read, write, and execute; group and others can read and execute |
| `devops.txt` | 444 | Read-only for everyone |
| `notes.txt` | 640 | Owner can read/write, group can read, others have no access |
| `project/` | 755 | Owner has full access; group and others can read and access the directory |

## Commands Used

**File Operations**

```bash
touch devops.txt
echo "I am learning Linux and DevOps." > notes.txt
vim script.sh
cat notes.txt
vim -R script.sh
```

**Reading Files**

```bash
head -n 5 /etc/passwd
tail -n 5 /etc/passwd
```

**Checking Permissions**

```bash
ls -l
ls -l devops.txt notes.txt script.sh
```

**Changing Permissions**

```bash
chmod +x script.sh
chmod a-w devops.txt
chmod 640 notes.txt
chmod 755 project
```

**Testing Permissions**

```bash
./script.sh
echo "Testing permissions" >> devops.txt
chmod a-x script.sh
./script.sh
```

## Challenges Faced

I learned that creating a file and executing a file are two different things.

Initially, a shell script does not necessarily have execute permission. I used `chmod +x` to make it executable.

I also tested writing to a read-only file and executing a script without execute permission. Linux denied these operations because of the permission settings.

## What I Learned

1. I learned how to create, read, and edit files using Linux commands.
2. I understood the Linux permission system: read (4), write (2), and execute (1).
3. I learned how to use `chmod` to control access to files and directories.

## Conclusion

This practical helped me understand how Linux manages file access and security.

File permissions are important in DevOps because they help protect sensitive files, control script execution, and prevent unauthorized modifications.