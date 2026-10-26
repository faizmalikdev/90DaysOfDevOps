# Day 09 Challenge

## Users & Groups Created

- **Users:** `tokyo`, `berlin`, `professor`, `nairobi`
- **Groups:** `developers`, `admins`, `project-team`

## Group Assignments

- `tokyo` → `developers`, `project-team`
- `berlin` → `developers`, `admins`
- `professor` → `admins`
- `nairobi` → `project-team`

## Directories Created

| Directory | Group Owner | Permissions |
|---|---|---|
| `/opt/dev-project` | `developers` | `775 (rwxrwxr-x)` |
| `/opt/team-workspace` | `project-team` | `775 (rwxrwxr-x)` |

Tested file creation as `tokyo` and `berlin` in `/opt/dev-project`, and as `nairobi` in `/opt/team-workspace`.

## Commands Used

```bash
# Create users
sudo useradd -m tokyo
sudo passwd tokyo
sudo useradd -m berlin
sudo passwd berlin
sudo useradd -m professor
sudo passwd professor
sudo useradd -m nairobi
sudo passwd nairobi

# Create groups
sudo groupadd developers
sudo groupadd admins
sudo groupadd project-team

# Assign users to groups
sudo usermod -aG developers tokyo
sudo usermod -aG developers,admins berlin
sudo usermod -aG admins professor
sudo usermod -aG project-team nairobi
sudo usermod -aG project-team tokyo

# Create shared directories and set permissions
sudo mkdir -p /opt/dev-project /opt/team-workspace
sudo chgrp developers /opt/dev-project
sudo chmod 775 /opt/dev-project
sudo chgrp project-team /opt/team-workspace
sudo chmod 775 /opt/team-workspace

# Test file creation
sudo -u tokyo touch /opt/dev-project/tokyo-file.txt
sudo -u berlin touch /opt/dev-project/berlin-file.txt
sudo -u nairobi touch /opt/team-workspace/nairobi-file.txt

# Verify users, groups, and permissions
grep -E '^(tokyo|berlin|professor|nairobi):' /etc/passwd
grep -E '^(developers|admins|project-team):' /etc/group
groups tokyo
groups berlin
groups professor
groups nairobi
ls -ld /opt/dev-project /opt/team-workspace
ls -l /opt/dev-project /opt/team-workspace
