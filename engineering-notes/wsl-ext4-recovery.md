# WSL ext4 Recovery Case Study

## Incident Summary

Environment

- Windows 11
- WSL2 Ubuntu
- ext4.vhdx (WSL virtual disk)
- MariaDB + ETL scripts

Accidental command executed:

rm -rf /home/dwkim_nethru/log

Deleted structure

log/
└ etl/
└ task/
├ script1.sh
├ script2.sh
├ script3.sh


Goal

Restore the deleted `log` directory with its original structure.

Result

- `log` directory recovery failed  
- other directories (`data`, `install`, `app`) successfully recovered

---

# WSL Filesystem Architecture

WSL2 does not run on a physical Linux disk.

Instead it stores the filesystem inside a **virtual disk (VHDX)**.

Windows
└ WSL2
└ ext4.vhdx
└ ext4 filesystem
└ /home/dwkim_nethru

Location of the virtual disk:

C:\Users<user>\AppData\Local\Packages...\ext4.vhdx

This file contains the entire Linux filesystem.

Therefore the following command affects the actual ext4 filesystem inside the virtual disk:

rm -rf

---

# How ext4 Deletes Files

From an ext4 filesystem perspective, deletion follows these steps.

1. directory entry removal  
2. inode link count decrement  
3. inode orphan processing  
4. block return to free list  

Important point:

File data blocks may still exist temporarily.

However the following metadata is removed immediately:

- filename
- directory structure
- path information

This makes directory reconstruction very difficult.

---

# Initial Recovery Strategy

Because extundelete requires an **unmounted filesystem**, the initial idea was:


WSL → ext4.vhdx
→ attach to Ubuntu VM
→ read-only mount


This is normally considered a safe recovery approach for ext4 filesystems.

---

# Why VM-Based Recovery Failed

Several issues occurred when attaching `ext4.vhdx` to a VM.

### 1. Dynamic VHDX

WSL disks are **dynamic VHDX files**.

When attached to a VM they may cause block mapping issues.

---

### 2. No partition table

Typical Linux disks:


MBR/GPT
└ partition
└ filesystem


WSL disks:


ext4 filesystem only


Some recovery tools fail to detect the filesystem correctly.

---

### 3. Recovery timing

Recovery success rate is highest **immediately after deletion**.

In this case several mount and scan operations occurred before recovery attempts.

During this time ext4 journal cleanup removed inode metadata.

---

# Final Recovery Method

The successful access method was:


wsl --mount ext4_backup.vhdx


WSL2 supports direct mounting of VHDX files.


WSL kernel
→ ext4 mount
→ /mnt/recover


In practice this approach is more compatible with WSL disks than VM-based recovery.

---

# Why Directory Recovery Failed

Recovery tools produced the following results.

| Tool | Result |
|-----|------|
| extundelete | failed |
| debugfs lsdel | no deleted inode |
| photorec | partial data recovery |

Interpretation:

ext4 journal had already committed metadata cleanup.

This removed the directory inode record.

Without inode metadata, directory structure reconstruction becomes impossible.

---

# Recovered Data

The following directories were successfully restored:


/home/dwkim_nethru/data
/home/dwkim_nethru/install
/home/dwkim_nethru/app


Reason:

These directories were not deleted.

They were simply copied after mounting the filesystem.

---

# Lessons Learned

## 1. `rm -rf` removes directory metadata immediately

Directory reconstruction is extremely difficult once metadata is removed.

---

## 2. extundelete only works immediately after deletion

Success conditions:

- filesystem unmounted
- journal intact
- no overwrite activity

---

## 3. WSL disks are best recovered using WSL tools


wsl --mount


proved more reliable than VM-based recovery.

---

## 4. Immediate response is critical

After accidental deletion the most important action is:

**stop the system immediately**

to prevent metadata overwrite.

---

# Prevention Strategy

## Automated backup


cron + tar backup


---

## WSL environment backup


wsl --export Ubuntu ubuntu_backup.tar


---

## Source control

ETL scripts should always be stored in a Git repository.

---

# Conclusion

In WSL ext4 filesystems, once journal metadata is cleaned up after a deletion caused by `rm -rf`, directory structure recovery becomes nearly impossible.

Therefore the most effective strategy is not recovery but prevention.

Backup and version control are essential safeguards.
