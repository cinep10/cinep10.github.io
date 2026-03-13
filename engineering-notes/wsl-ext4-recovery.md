# WSL ext4 Recovery Case Study


## Incident Summary

### Environment

- Windows 11
- WSL2 Ubuntu
- `ext4.vhdx` (WSL virtual disk)
- MariaDB + ETL scripts

### Accidental command executed

```bash
rm -rf /home/dwkim_nethru/log
```

### Deleted structure

```text
log/
└── etl/
    └── task/
        ├── script1.sh
        ├── script2.sh
        └── script3.sh
```

### Goal

Restore the deleted `log` directory with its original structure.

### Result

- `log` directory recovery failed
- other directories such as `data`, `install`, and `app` were recovered successfully

---

## WSL Filesystem Architecture

WSL2 does not run on a physical Linux disk.

Instead, it stores the Linux filesystem inside a virtual disk file.

```text
Windows
└── WSL2
    └── ext4.vhdx
        └── ext4 filesystem
            └── /home/dwkim_nethru
```

Typical location of the virtual disk:

```text
C:\Users\<user>\AppData\Local\Packages\...\ext4.vhdx
```

This file contains the full Linux filesystem used by WSL.

That means file deletion happens inside the actual ext4 filesystem stored in the VHDX file.

---

## How `rm -rf` Works in ext4

From an ext4 filesystem perspective, deletion usually follows these steps:

1. remove directory entry
2. decrease inode link count
3. mark inode as orphan
4. return blocks to the free list

Important point:

- file data blocks may still remain temporarily
- but filename, path, and directory metadata are removed first

This is why recovering the original directory structure is much harder than recovering raw file content.

---

## Initial Recovery Strategy

Because `extundelete` requires an **unmounted filesystem**, the initial recovery idea was:

```text
WSL
└── ext4.vhdx
    └── attach to Ubuntu VM
        └── mount read-only
```

The goal was to recover the ext4 filesystem in a safer and more standard Linux recovery environment.

---

## Why VM-Based Recovery Failed

### 1. Dynamic VHDX behavior

WSL disks use a dynamic VHDX format.

When attached to a VM, this may introduce block mapping or device recognition issues.

### 2. No partition table

Typical Linux disks often look like this:

```text
MBR/GPT
└── partition
    └── filesystem
```

WSL disks are often closer to this:

```text
ext4 filesystem directly inside VHDX
```

This can confuse recovery tools that expect a normal partition layout.

### 3. Recovery timing

Recovery success is highest immediately after deletion.

In this case, several mount and scan operations happened before recovery was completed.

During that process, ext4 journal cleanup likely removed inode metadata.

---

## Final Recovery Method

The most practical access path was:

```bash
wsl --mount ext4_backup.vhdx
```

This allowed the disk to be mounted directly inside WSL.

```text
WSL kernel
└── ext4 mount
    └── /mnt/recover
```

In practice, this worked more reliably than the VM-based approach for a WSL disk.

---

## Why Directory Recovery Failed

The recovery tools behaved as follows:

- `extundelete` → failed
- `debugfs lsdel` → no deleted inode found
- `photorec` → partial raw file recovery possible

Interpretation:

- ext4 journal had already committed metadata cleanup
- deleted directory inode information was no longer available
- without inode metadata, original directory structure could not be reconstructed

---

## Recovered Data

The following directories were recovered successfully:

```text
/home/dwkim_nethru/data
/home/dwkim_nethru/install
/home/dwkim_nethru/app
```

Reason:

These directories were not deleted.  
They were simply copied after mounting the filesystem.

---

## Lessons Learned

### 1. `rm -rf` removes directory metadata immediately

Directory reconstruction becomes very difficult once metadata is gone.

### 2. `extundelete` is time-sensitive

It works best only when:

- the filesystem is unmounted
- the journal is still intact
- overwrite activity has not occurred

### 3. WSL disks are best handled with WSL-compatible tooling

For this case, `wsl --mount` was more reliable than attaching the disk to a VM.

### 4. Immediate response matters most

After accidental deletion, the most important step is:

**stop further write activity immediately**

---

## Prevention Strategy

### 1. Backup automation

Use scheduled archive backups such as:

```text
cron + tar backup
```

### 2. Full WSL backup

```bash
wsl --export Ubuntu ubuntu_backup.tar
```

### 3. Source control

ETL scripts should always be stored in a Git repository.

---

## Conclusion

In WSL ext4 filesystems, once journal metadata is cleaned up after an `rm -rf` deletion, directory structure recovery becomes nearly impossible.

The real lesson from this case is simple:

**recovery is difficult, prevention is cheaper**

Reliable engineering starts with backup and version control, not recovery tooling.
