# Day 13 – Linux Volume Management (LVM)

## Objective
Learn how to manage Linux storage using Logical Volume Manager (LVM), including creating, extending, formatting, and mounting logical volumes.

## 1. Understanding LVM

LVM (Logical Volume Manager) provides flexible storage management in Linux. It allows us to combine physical storage devices and allocate storage dynamically.

### LVM Architecture

1. **Physical Volume (PV):** A physical disk or partition prepared for LVM.
2. **Volume Group (VG):** A storage pool created by combining physical volumes.
3. **Logical Volume (LV):** A virtual storage volume allocated from a volume group.
4. **Filesystem:** A format such as ext4 that allows Linux to store files.

**Storage Flow:**

```text
Physical Disk → Physical Volume (PV) → Volume Group (VG) → Logical Volume (LV) → Filesystem → Mount Point
```

## 2. Important LVM Commands

| Command | Purpose |
| :--- | :--- |
| `lsblk` | Lists disks and partitions |
| `pvs` | Displays physical volumes |
| `pvcreate` | Creates a physical volume |
| `vgs` | Displays volume groups |
| `vgcreate` | Creates a volume group |
| `lvs` | Displays logical volumes |
| `lvcreate` | Creates a logical volume |
| `lvextend` | Extends a logical volume |
| `mkfs.ext4` | Creates an ext4 filesystem |
| `resize2fs` | Resizes an ext4 filesystem |
| `mount` | Mounts a filesystem |
| `df -h` | Displays disk usage |

## 3. Practical Tasks

### Task 1: Check Current Storage

```bash
sudo -i
lsblk
pvs
vgs
lvs
df -h
```

### Task 2: Create Physical Volume

```bash
pvcreate /dev/sdb
pvs
```

### Task 3: Create Volume Group

```bash
vgcreate devops-vg /dev/sdb
vgs
```

### Task 4: Create Logical Volume

```bash
lvcreate -L 500M -n app-data devops-vg
lvs
```

### Task 5: Format and Mount

```bash
mkfs.ext4 /dev/devops-vg/app-data
mkdir -p /mnt/app-data
mount /dev/devops-vg/app-data /mnt/app-data
df -h /mnt/app-data
```

### Task 6: Extend the Logical Volume

```bash
lvextend -L +200M /dev/devops-vg/app-data
resize2fs /dev/devops-vg/app-data
df -h /mnt/app-data
```

## 4. What I Learned

1. LVM makes Linux storage management flexible by using Physical Volumes, Volume Groups, and Logical Volumes.
2. Logical volumes can be extended when additional storage is required.
3. Filesystems must be created and mounted before storing files on a logical volume.

## 5. Important Notes

- Always check available disks using `lsblk` before creating a physical volume.
- Never run `pvcreate` on a disk containing important data.
- `lvextend` increases the logical volume size.
- `resize2fs` expands the ext4 filesystem to use the additional space.
- A logical volume can be mounted to a directory such as `/mnt/app-data`.

