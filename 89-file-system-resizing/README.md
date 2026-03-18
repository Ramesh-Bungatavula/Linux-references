# 📦 Linux Storage Management Guide (lsblk, df, Filesystem & LVM Resizing)

This guide covers essential Linux storage concepts and commands:

- `lsblk` (block devices & filesystems)  
- `df -h` (disk usage)  
- Filesystem resizing (EXT4/XFS)  
- LVM resizing (PV, VG, LV)  

---

# 🔹 1. lsblk — List Block Devices

## 📌 Purpose
Displays all block devices (disks, partitions, mount points, filesystems).

## 📌 Command
```bash
lsblk
```

## 📌 Example Output
```
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  20G  0 disk 
├─xvda1 202:1    0  20G  0 part /
```

## 📌 Useful Variations
```bash
lsblk -f
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINT
```

## 📌 Key Info
- `disk` → physical device  
- `part` → partition  
- `MOUNTPOINT` → where it is mounted  
- `FSTYPE` → filesystem (ext4, xfs)  

---

# 🔹 2. df -h — Disk Usage

## 📌 Purpose
Shows disk space usage of mounted filesystems.

## 📌 Command
```bash
df -h
```

## 📌 Example Output
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       20G  8.0G   11G  42% /
```

## 📌 Key Info
- `Size` → total space  
- `Used` → used space  
- `Avail` → free space  
- `Use%` → usage percentage  

---

# 🔹 3. Filesystem Resizing (EC2 / Cloud Scenario)

## 🎯 Scenario
Disk size increased (e.g., 20GB → 40GB), but OS still shows old size.

---

## ✅ Step 1: Install tool
```bash
sudo apt install cloud-guest-utils -y
```

---

## ✅ Step 2: Check disk
```bash
lsblk
```

---

## ✅ Step 3: Expand partition
```bash
sudo growpart /dev/xvda 1
```

---

## ✅ Step 4: Resize filesystem

### 🟢 EXT4
```bash
sudo resize2fs /dev/xvda1
```

### 🔵 XFS
```bash
sudo xfs_growfs /
```

---

## ✅ Step 5: Validate
```bash
lsblk
df -h
```

---

# 🔹 4. LVM (Logical Volume Manager)

## 📌 Architecture
```
Disk → Partition → PV → VG → LV → Filesystem → Mount
```

| Layer | Description |
|------|------------|
| PV | Physical Volume |
| VG | Volume Group |
| LV | Logical Volume |

---

## 🔹 Check LVM Setup
```bash
pvs
vgs
lvs
```

---

# 🔹 5. LVM Resizing (Expand Existing Disk)

## 🎯 Scenario
Disk increased in cloud (e.g., EC2)

---

## ✅ Step 1: Verify disk
```bash
lsblk
```

---

## ✅ Step 2: Resize partition
```bash
sudo growpart /dev/xvda 1
```

---

## ✅ Step 3: Resize PV
```bash
sudo pvresize /dev/xvda1
```

---

## ✅ Step 4: Check free space
```bash
vgdisplay
```

---

## ✅ Step 5: Extend LV

### Use all free space
```bash
sudo lvextend -l +100%FREE /dev/vg0/root
```

### Add specific size
```bash
sudo lvextend -L +10G /dev/vg0/root
```

---

## ✅ Step 6: Resize filesystem

### EXT4
```bash
sudo resize2fs /dev/vg0/root
```

### XFS
```bash
sudo xfs_growfs /
```

---

## ✅ Step 7: Validate
```bash
df -h
lsblk
```

---

# 🔹 6. One-Line LVM Resize (Recommended)

```bash
sudo lvextend -r -l +100%FREE /dev/vg0/root
```

👉 `-r` automatically resizes filesystem  

---

# 🔹 7. Add New Disk to LVM

## 🎯 Scenario: New disk `/dev/xvdb`

```bash
# Create PV
sudo pvcreate /dev/xvdb

# Extend VG
sudo vgextend vg0 /dev/xvdb

# Extend LV
sudo lvextend -l +100%FREE /dev/vg0/root

# Resize filesystem
sudo resize2fs /dev/vg0/root
```

---

# 🔹 8. Shrinking LVM (⚠️ Risky)

⚠️ Only supported safely with EXT4  

```bash
sudo umount /dev/vg0/root
sudo e2fsck -f /dev/vg0/root
sudo resize2fs /dev/vg0/root 10G
sudo lvreduce -L 10G /dev/vg0/root
```

👉 Always take backup before shrinking  

---

# 🔹 9. Key Commands Cheat Sheet

| Task | Command |
|------|--------|
| List disks | `lsblk` |
| Disk usage | `df -h` |
| Show PV | `pvs` |
| Show VG | `vgs` |
| Show LV | `lvs` |
| Resize PV | `pvresize` |
| Extend LV | `lvextend` |
| Reduce LV | `lvreduce` |

---

# 🔹 10. Important Notes

- Always follow order:
  ```
  Disk → Partition → PV → VG → LV → Filesystem
  ```

- EXT4 → `resize2fs`  
- XFS → `xfs_growfs`  

- Cloud resize requires OS-level resize  

---

# ✅ Summary

| Tool | Purpose |
|------|--------|
| lsblk | View disk structure |
| df -h | Check disk usage |
| growpart | Expand partition |
| resize2fs | Resize EXT4 filesystem |
| pvresize | Resize LVM physical volume |
| lvextend | Extend logical volume |

---
