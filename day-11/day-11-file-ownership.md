# Day 11 Challenge – File Ownership

## Files & Directories Created

### Files
- `devops-files.txt`
- `team-notes.txt`
- `project-config.yaml`
- `heist-project/vault/gold.txt`
- `heist-project/plans/strategy.conf`
- `bank-heist/access-codes.txt`
- `bank-heist/blueprints.pdf`
- `bank-heist/escape-plan.txt`

### Directories
- `app-logs/`
- `heist-project/`
- `heist-project/vault/`
- `heist-project/plans/`
- `bank-heist/`

### Users Created
- `tokyo`
- `berlin`
- `nairobi`

### Groups Created
- `malik-team`
- `planners`
- `vault-team`
- `tech-team`

## Ownership Changes

| File / Directory | Before (Owner:Group) | After (Owner:Group) |
|---|---|---|
| `devops-files.txt` | `faiz_malik:faiz_malik` | `prince:faiz_malik` |
| `team-notes.txt` | `faiz_malik:faiz_malik` | `faiz_malik:malik-team` |
| `project-config.yaml` | `faiz_malik:faiz_malik` | `sana:malik-team` |
| `app-logs/` | `faiz_malik:faiz_malik` | `prince:malik-team` |
| `heist-project/` and its contents | `faiz_malik:faiz_malik` | `prince:planners` |
| `bank-heist/access-codes.txt` | `faiz_malik:faiz_malik` | `tokyo:vault-team` |
| `bank-heist/blueprints.pdf` | `faiz_malik:faiz_malik` | `berlin:tech-team` |
| `bank-heist/escape-plan.txt` | `faiz_malik:faiz_malik` | `nairobi:vault-team` |

## Commands Used

### 1. Check Existing Ownership

```bash
ls -l
```
### 2. Create a File and Change Its Owner

Create a new file and transfer its user ownership between `sana` and `prince`.

```bash
touch devops-files.txt
sudo chown sana devops-files.txt
ls -l devops-files.txt

sudo chown prince devops-files.txt
ls -l devops-files.txt
```

---

### 3. Change Group Ownership with `chgrp`

Create a new group and assign group ownership of a file without modifying its user owner.

```bash
touch team-notes.txt
sudo groupadd malik-team
sudo chgrp malik-team team-notes.txt
ls -l team-notes.txt
```

---

### 4. Change Owner and Group Together

Update both the user owner and group owner simultaneously using `user:group` syntax for files and directories.

```bash
# For a file
touch project-config.yaml
sudo chown sana:malik-team project-config.yaml
ls -l project-config.yaml

# For a directory
mkdir app-logs/
sudo chown prince:malik-team app-logs/
ls -ld app-logs/
```

---

### 5. Change Ownership Recursively

Apply ownership changes to a parent directory and all nested subdirectories and files using the `-R` flag.

```bash
mkdir -p heist-project/vault
mkdir -p heist-project/plans
touch heist-project/vault/gold.txt
touch heist-project/plans/strategy.conf

sudo groupadd planners
sudo chown -R prince:planners heist-project/
ls -lR heist-project/
```

---

### 6. Practice Challenge

Set up multiple users, groups, and project files with specific ownership assignments.

```bash
# Create users and groups
sudo useradd tokyo
sudo useradd berlin
sudo useradd nairobi
sudo groupadd vault-team
sudo groupadd tech-team

# Create project structure
mkdir bank-heist
touch bank-heist/access-codes.txt
touch bank-heist/blueprints.pdf
touch bank-heist/escape-plan.txt

# Assign ownerships
cd bank-heist/
sudo chown -R tokyo:vault-team access-codes.txt
cd ..

sudo chown berlin:tech-team bank-heist/blueprints.pdf
sudo chown nairobi:vault-team bank-heist/escape-plan.txt

# Verify ownerships
ls -l bank-heist/
ls -ld bank-heist/
```

---

### Actual Final Output

#### Recursive Ownership – `heist-project/`

```text
heist-project/:
total 8
drwxr-xr-x 2 prince planners 4096 Nov 16 07:30 plans
drwxr-xr-x 2 prince planners 4096 Nov 16 07:29 vault

heist-project/plans:
total 0
-rw-r--r-- 1 prince planners 0 Nov 16 07:30 strategy.conf

heist-project/vault:
total 0
-rw-r--r-- 1 prince planners 0 Nov 16 07:29 gold.txt
```

### Final Ownership – `bank-heist/`

```text
total 0
-rw-r--r-- 1 tokyo   vault-team 0 Nov 16 07:40 access-codes.txt
-rw-r--r-- 1 berlin  tech-team  0 Nov 16 07:40 blueprints.pdf
-rw-r--r-- 1 nairobi vault-team 0 Nov 16 07:40 escape-plan.txt
```

### Directory Ownership

```text
drwxr-xr-x 2 faiz_malik faiz_malik 4096 Nov 16 07:40 bank-heist/
```

---

### What I Learned

1. **`chown`** changes the owner of a file or directory.
2. **`chgrp`** changes the group ownership without changing the user owner.
3. **`chown -R`** changes ownership recursively for a directory and everything inside it.
4. The syntax **`sudo chown user:group filename`** changes both the owner and group at the same time.
5. **`sudo`** is generally required to change a file's ownership to another user.
6. **Relative paths** depend on the current working directory; moving into the target directory resolved a `chown` path error.
