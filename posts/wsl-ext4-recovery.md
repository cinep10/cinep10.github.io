# WSL ext4 Recovery Case Study

## Incident

While working in a WSL Ubuntu environment, I accidentally executed:

`rm -rf /home/user/log`

This removed an ETL-related script directory.

## Filesystem context

WSL2 stores Linux files inside a virtual disk:

Windows host  
→ WSL2  
→ ext4.vhdx  
→ ext4 filesystem  

## What I learned

Deleting files in ext4 does not immediately erase all data.

Typical sequence:

- directory entry removed
- inode link count decreased
- inode metadata orphaned
- blocks returned to free list

This means raw file content may remain temporarily, but directory structure is often lost first.

## Recovery attempts

- backup of ext4.vhdx
- extundelete
- debugfs inode scan
- photorec raw scan

## Result

Some file content was partially recoverable, but the original directory structure was not.

## Engineering lesson

Recovery is difficult. Prevention is cheaper.

After this incident, I changed my workflow:

- ETL scripts moved into Git
- WSL backup added
- periodic archive backup added
