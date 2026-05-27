# Linux Basics

## diff command
- d → line present in first file but not in second
- < → from first file
- > → from second file

Example:
1,75d0 → lines 1–75 deleted from first file

---

## Process States
- R → Running
- S → Sleeping (interruptible)
- S+ → Foreground process
- T → Stopped (Ctrl+Z)
- Z → Zombie (completed but not cleaned)
- D → Uninterruptible sleep (I/O wait)

## sort command

- sort → sorts lines alphabetically or numerically
- sort -r → reverse order (descending)
- sort -n → numeric sort

Example:
sort numbers.txt
sort -r numbers.txt
sort -n numbers.txt

Note:
sort -r does reverse lexicographic order, not numeric.
That’s why:
634 comes before 6123 (string comparison)

---

## tee command

- tee → writes output to both terminal and file

Examples:

echo "hello" | tee file.txt
# writes "hello" to file.txt and shows on terminal

ls | tee list.txt
# saves ls output and prints it

echo "new line" | tee -a file.txt
# -a means append (don’t overwrite)

## 📅 17-04-2026 — Log Analysis Practice

### 🔸 Topics Covered
- awk (column extraction, filtering)
- grep (log filtering)
- head (first lines)
- sed (line range extraction)

---

### 🔸 Commands Practiced

- awk '/INFO/' sample.log
- grep -c "INFO" sample.log
- awk '{print $1,$2,$4}' sample.log
- head -n 10 sample.log
- sed -n '3,10p' sample.log | awk '{print $2,$3,$5}'

---

### 🔸 Key Learnings
- awk uses `$` for columns (`$1,$2...`)
- single quotes `' '` must be used in awk
- default delimiter is space
- pipelines `|` combine commands

---

### 🔸 Mistakes Faced
- used double quotes `" "` → caused awk error
- forgot `$` before column numbers → printed literals

---

### 🔸 Summary
Practiced real log analysis using Linux commands on sample.log and improved understanding of awk behavior and shell quoting.

## Storage & LVM

- Difference between xvda and nvme (AWS Nitro vs Xen)
- lsblk: list block devices
- df -h: disk usage
- Identifying root disk using lsblk (/ mount point)
- nvme naming: nvmeXn1, partitions as p1
- Raw disk vs PV vs filesystem

### LVM
- pvcreate: create physical volume
- pvs: list PVs
- vgcreate, lvcreate (overview)

### Important
- Never run pvcreate on root disk (nvme0n1)

## sed & awk
- NR usage in awk
- Print specific lines and columns
- sed -n and p flag behavior

## tmux
- sessions, windows, attach/detach
- why tmux is used in DevOps

feat: add tmux basics (sessions, split panes, navigation, config)

- learned tmux start, attach, detach, list sessions
- practiced vertical (%) and horizontal (") splits
- navigation using prefix + arrow keys
- closing panes and session behavior
- created .tmux.conf and changed prefix key
- understood real use (monitor + run commands in parallel)


## Revision - 20 Apr (with practical)

### File & Directory
ls, cd, pwd, mkdir, rm, rmdir, touch, cp, mv

### File Viewing
cat, zcat, head, tail -f, less, more

### Text Processing
wc, cut, sort, diff, tee

### Links
ln (hard link, soft link)

### System / Process
ps, top, kill, nohup, free, vmstat

### Disk
df, du

### Remote Access
ssh

### Editor
vi basics

## Revision (Day 2)
- Reviewed all previously learned Linux commands
- Practiced scenario-based usage:
  - file search + filtering
  - permissions handling
  - process management

## tmux

### Purpose

* Multiple terminals in one session
* Prevent process loss after SSH disconnect

---

### Session Commands

```bash
tmux new -s dev
tmux ls
tmux attach -t dev
tmux kill-session -t dev
tmux kill-server
```

---

### Window Shortcuts

```bash
Ctrl + b c   # new window
Ctrl + b n   # next window
Ctrl + b p   # previous window
Ctrl + b &   # kill window
```

---

### Pane Shortcuts

```bash
Ctrl + b %   # vertical split
Ctrl + b "   # horizontal split
Ctrl + b arrows  # move between panes
```

---

### Detach / Attach

```bash
Ctrl + b d
tmux attach -t <name>
```

---

### Common Errors

* nested session → already inside tmux → use `Ctrl + b d`
* attach error → use `tmux attach -t <name>`
* kill session → must use `-t`

---

### Minimal Config (~/.tmux.conf)

```bash
set -g mouse on
bind r source-file ~/.tmux.conf \; display "Reloaded!"
```

---

## ss command

### Purpose

* Check ports and processes

---

### Commands

```bash
ss -lunp   # UDP ports
ss -ltnp   # TCP ports
ss -tulnp  # all ports
```

---

### Options

* l → listening
* u → UDP
* t → TCP
* n → numeric
* p → process

---

### Use Case

* Check running services
* Debug port issues

---

## Editors (nano & vim)

### nano

```bash
Ctrl + O  # save
Enter     # confirm
Ctrl + X  # exit
```

---

### vim

```bash
i        # insert mode
Esc      # normal mode
:wq      # save & exit
:q!      # exit without saving

## Linux Practice (Day)

### File Transfer
- scp: upload/download files using key
- rsync:
  - sync folders efficiently
  - --dry-run for preview
  - trailing / behavior (folder vs contents)
  - difference between scp and rsync

### Disk & Storage
- lsblk vs df:
  - lsblk → structure (disks, partitions)
  - df → usage (space)
- identified nvme disks (system vs free disks)
- mount concept (attach disk to directory)

### LVM
- PV → VG → LV workflow
- created and understood logical volumes
- extend volume and resize filesystem

### Permissions
- chmod usage (400 for .pem key)
- executable bit (x) and file color change

### sed (log processing)
- print matching lines: sed -n '/info/p'
- case-insensitive: /info/I
- print line number: {=;p}
- use of -n, p, =, I flags
feat: practice Linux storage management and advanced command revision

* Practiced full LVM lifecycle on EC2:

  * pvcreate, vgcreate, lvcreate
  * filesystem creation with mkfs.ext4
  * mounting logical volume and verification using df -h and lsblk
  * permanent mount understanding with /etc/fstab and mount -a
  * full rollback using umount, lvremove, vgremove, pvremove

* Revised Linux storage concepts:

  * PV, VG, LV architecture and LVM workflow
  * mount points, /data vs /root/data difference
  * /dev directory, disk naming (/dev/sda, /dev/nvme*)
  * lost+found directory purpose
  * root vs normal user prompt ($ vs #)

* Practiced troubleshooting:

  * resolved "target is busy" during umount using fuser and kill
  * understood process handling for mounted filesystems

* Revised networking and text-processing commands:

  * nc -zv
  * ss -lunp
  * sed pattern matching, substitution, line numbers, ranges
  * awk count patterns
  * tmux kill-session

- revised sed command syntax and range usage
- practiced awk vs sed differences
- fixed scp directory copy using -r and SSH key path issues
- revised rsync authentication and key troubleshooting
- practiced curl GET requests, curl -X GET, and curl -s usage
- revised ss -lunp for UDP listening ports
- understood ps aux vs ps -ef option styles
- completed LVM full lifecycle revision (PV → VG → LV → mount → fstab → rollback)
- revised permanent mount using /etc/fstab and UUID

## Content to append in notes file

### Linux & DevOps Practice – 26-04-26

* Revised complete LVM lifecycle:
  PV → VG → LV → Filesystem → Mount → Extend → Reduce → Remove

* Practiced:

  * `pvcreate`
  * `vgcreate`
  * `lvcreate`
  * `lvextend`
  * `resize2fs`
  * `lvreduce`
  * `lvremove`
  * `vgremove`
  * `pvremove`

* Learned:

  * `grep` → searching
  * `awk` → field-based processing
  * `sed` → stream editing

* Practiced:

  * `wget` vs `curl`
  * `scp` syntax and SSH key troubleshooting
  * `nc -zv` for port testing
  * `telnet` vs `nc`

* AWS revision:

  * EBS must be attached in same Availability Zone as EC2
  * command to check EC2 AZ:
    `curl http://169.254.169.254/latest/meta-data/placement/availability-zone`

* Fixed WSL issue:

  * `.profile` was not loading `.bashrc`
  * added:
    `if [ -f ~/.bashrc ]; then . ~/.bashrc; fi`

* Fixed tmux:

  * mouse support
  * clipboard support
  * persistent config via `.tmux.conf`


## Git Revision - Branching, Logs, Tags, Rebase

Revised important Git commands for DevOps:

- branching and switching branches
- merge and rebase workflow
- local and remote branch inspection
- commit history inspection using log/show/diff/blame
- tag creation and pushing tags
- reflog for recovery
- pull request and fork workflow basics
- prune remote references
- archive and advanced fetch usage

feat: Linux swap management hands-on practice and LVM snapshot basics

* practiced complete Linux swap management workflow from creation to removal
* checked RAM and swap usage using free -h and swapon --show
* created swap file using fallocate and dd methods
* understood dd command structure (if, of, bs, count) and size calculation
* changed swapfile permissions using chmod 600 and learned security reason behind it
* converted swapfile to swap area using mkswap
* enabled and disabled swap using swapon and swapoff
* verified active swap and understood PRIO (swap priority) concept
* configured persistent swap using /etc/fstab
* learned /swapfile none swap sw 0 0 entry breakdown
* tested fstab safely using mount -a and mount -av
* learned correct swap removal workflow
* revised interview scenarios for memory full and inactive swap cases
* understood fdisk -l usage and Linux LVM partition type 8e concept
* started LVM snapshot basics and production use cases
* learned why snapshots are used before risky upgrades, deployments, and patches

## LVM Snapshot – Hands-on Practice

### Setup
- Created new VG `3n1data-vg` using free disk
- Created LV `lv3` (10G) for lab
- Formatted with ext4 and mounted on `/snaplab`

### Snapshot Creation
- Created snapshot `snap1` (2G) of `lv3`
- Understood snapshot requires free VG space (cannot use 100%FREE)

### COW (Copy-on-Write)
- Snapshot stores only changed blocks, not full data
- On modification:
  - Old data → copied to snapshot
  - New data → written to LV

### Recovery Practice
- Deleted file from LV
- Mounted snapshot in read-only mode
- Recovered file using:
  cp /mnt/snap/file1.txt /snaplab/

### Observations
- `lvs` shows snapshot relation with Origin LV
- `Data%` indicates snapshot usage
- Snapshot becomes invalid if it reaches 100%

### Key Commands
pvcreate
vgcreate / vgextend
lvcreate
lvcreate -s (snapshot)
lvs
mount -o ro
cp (recovery)

## LVM Snapshot – Clean Setup (From Scratch)

### Environment Reset
- Removed old LVM setup (lv3, data-3lv confusion cleared)
- Used fresh disk `/dev/nvme3n1` for clean lab

### LVM Setup
- Created Physical Volume (PV)
  pvcreate /dev/nvme3n1

- Created Volume Group (VG)
  vgcreate lab-vg /dev/nvme3n1

- Created Logical Volume (LV)
  lvcreate -L 10G -n lab-lv lab-vg

- Formatted and mounted
  mkfs.ext4 /dev/lab-vg/lab-lv
  mount /dev/lab-vg/lab-lv /lab

### Snapshot Workflow (Correct Order)
1. Created file BEFORE snapshot
2. Created snapshot (lab-snap)
3. Mounted snapshot as read-only
4. Modified original file
5. Compared live vs snapshot data

### Key Observation
- /lab → shows current data
- /mnt/snap → shows old (snapshot) data
- Snapshot only captures data present at creation time

### COW (Copy-on-Write)
- Old data copied to snapshot before modification
- New data written to original LV
- Snapshot reconstructs old state using COW

### Common Mistakes Fixed
- Using wrong paths (/lv3 vs /data-lv3)
- Creating file after snapshot
- Mount option typo (-o -ro)
- Not verifying mount points

### Commands Practiced
pvcreate, vgcreate, lvcreate
mkfs.ext4, mount
lvcreate -s (snapshot)
lvs, df -h
tee, cat

# LVM Snapshot - Recovery & Failure

## Snapshot behavior
- Snapshot stores old data using copy-on-write (COW)
- New files after snapshot are not visible in snapshot
- Modified/deleted files remain unchanged in snapshot

## File-level recovery
- Mount snapshot
- Copy required file from snapshot to original LV

## Full rollback
- Unmount original and snapshot
- Merge snapshot into original LV
- Reactivate or reboot
- Mount again

## Snapshot limitation
- Snapshot size is limited (COW)
- If COW becomes full → snapshot becomes invalid
- Cannot use snapshot after overflow

---

# Swap Management

## Check swap
free -h
swapon --show

## Create swap file
fallocate -l 1G /swapfile

## Permissions
chmod 600 /swapfile

## Setup swap
mkswap /swapfile

## Enable swap
swapon /swapfile

## Make persistent
/etc/fstab entry:
/swapfile none swap sw 0 0

## Disable swap
swapoff /swapfile

## Remove swap
rm /swapfile


Linux: Swap, LVM Snapshot, Process Management + Disk Practice

- Practiced swap management:
  - Created and enabled swap file
  - Understood swappiness and swap priority
  - Used swapon / swapoff

- Worked on LVM:
  - Learned PV → VG → LV flow
  - Practiced LV extend and reduce
  - Understood importance of e2fsck before shrinking
  - Studied LVM snapshots (concept + use cases)

- Performed disk operations:
  - Partitioned disk using fdisk
  - Handled existing filesystem signature
  - Created ext4 filesystem
  - Mounted disk and updated /etc/fstab
  - Debugged "device in use" (fdisk process issue)

- Practiced filesystem tools:
  - Used e2fsck for filesystem check
  - Learned resize2fs for resizing

- Learned process management:
  - Used ps, top, pgrep
  - Understood process states (S, S+, T)
  - Practiced bg, fg, jobs
  - Learned kill vs kill -9
  - Identified high CPU processes using top


Added Linux swap management practice
Practiced process management commands and debugging
Completed LVM snapshot lab (create, mount, merge, restore)

# 📘 Linux Learning Notes — 05 May 2026

## 🔹 Process Management

### Process States

* **Foreground**: Runs in terminal, blocks it
* **Background**: Runs without blocking terminal
* **D State**: Uninterruptible sleep (waiting for I/O, cannot be killed)
* **Zombie (Z State)**: Process finished but not cleaned (parent didn’t call `wait()`)

### Signals

* **SIGTERM (15)** → graceful kill (safe shutdown)
* **SIGKILL (9)** → force kill (immediate stop)

### Commands

```bash
ps aux
pgrep <process>
kill <PID>
kill -9 <PID>
jobs
fg
bg
```

---

## 🔹 Swap Management

### Create Swap File

```bash
fallocate -l 1G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```

### Check Usage

```bash
free -h
swapon --show
```

### Concept

* Swap = disk used as memory
* **Swappiness** controls how aggressively swap is used

---

## 🔹 LVM (Logical Volume Management)

### Check Space

```bash
vgs
vgdisplay
lvs
```

### Key Concepts

* VG = pool of storage
* LV = logical partition
* PE = smallest allocation unit

---

### Extend LV (Safe)

```bash
lvextend -r -L +2G /dev/data-vg/data-lv
```

---

### Reduce LV (Important Order)

```bash
umount
e2fsck -f
resize2fs (new size)
lvreduce
mount
```

⚠️ Always shrink filesystem before reducing LV

---

## 🔹 LVM Snapshot

### Create Snapshot

```bash
lvcreate -L 1G -s -n snap1 /dev/data-vg/data-lv
```

### Merge Snapshot (Rollback)

```bash
lvconvert --merge /dev/data-vg/snap1
```

### Concept

* Uses **Copy-on-Write (COW)**
* Stores only changed data

### Snapshot Full

* Snapshot becomes invalid
* Cannot be used for recovery

---

## 🔹 Disk & I/O Troubleshooting

### Check Disk Usage

```bash
df -h
lsblk
```

### Check Disk Performance

```bash
iostat
```

### Key Metric

* **%iowait** → high = disk bottleneck

---

## 🔹 Troubleshooting Mindset

Problems are categorized into:


* CPU → `top`
* Memory → `free`, `vmstat`
* Disk → `df`, `iostat`
* Process → `ps`, `kill`
* Logs → `journalctl`

---

## 🎯 Key Takeaways

* Always follow correct order in LVM operations
* Zombie processes occur due to missing `wait()`
* Use `iostat` to detect disk I/O issues
* Snapshot size depends on write activity
* D state processes cannot be killed

# 06 May 2026

## tmux
- Terminal multiplexer for persistent sessions
- `tmux new -s session`
- `tmux ls`
- `tmux attach -t session`
- `Ctrl+b d` → detach session

## Swap
- Swap = disk-based virtual memory
- Swap setup flow:
  create file → chmod 600 → mkswap → swapon
- `free -h`
- `swapon --show`

## LVM Snapshot
- Snapshot already contains filesystem/data
- Snapshot does NOT need `mkfs.ext4`
- Snapshot merge restores old LV state

## Important Learning
- Unmounted ≠ inactive
- Same LV can have multiple mount points
- All mount points must be unmounted before immediate snapshot merge

## Useful Commands
```bash
sudo lvcreate -L 1G -s -n data-snap /dev/data-vg/data-lv
sudo lvconvert --merge /dev/data-vg/data-snap
sudo lvchange -an /dev/data-vg/data-lv
sudo lvchange -ay /dev/data-vg/data-lv

# 📘 Linux Notes — 07 May 2026

## SCP

```bash
scp user@server:/path/file .
scp -i ~/key.pem ubuntu@server:/home/ubuntu/file.txt .
scp -i ~/key.pem -r ubuntu@server:/home/ubuntu/project .
```

## REST API

```bash
curl https://jsonplaceholder.typicode.com/users
```

## Hostname

```bash
hostname
```

## LVM

```bash
sudo lvcreate -l 100%FREE -n data-lv data-vg
sudo lvs
sudo lvremove /dev/data-vg/data-lv
sudo vgremove data-vg
```

## LVM Rollback

```bash
sudo umount /mnt/data
sudo lvremove /dev/data-vg/data-lv
sudo vgremove data-vg
sudo pvremove /dev/xvdf
```

## FSTAB Recovery

```bash
sudo cp /etc/fstab.backup /etc/fstab
sudo mount -a
```

## Swap

```bash
free -h
sudo swapon --show
sudo fallocate -l 1G /swapfile
sudo dd if=/dev/zero of=/swapfile bs=1M count=1024
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo swapoff /swapfile
cat /proc/sys/vm/swappiness
sudo sysctl vm.swappiness=10
```

## Process Management

```bash
ps aux
htop
pgrep nginx
kill 1234
kill -9 1234
pkill -9 nginx
jobs
fg
bg
## Users & Groups

- `useradd -m user` → create user with home directory
- `passwd user` → set/change password
- `passwd -d user` → remove password
- `usermod -aG group user` → add user to group
- `passwd -S user` → check password status
- `/etc/passwd` → user info
- `/etc/shadow` → password hashes
- `/etc/group` → group info
- UID >=1000 usually normal users, lower UIDs mostly system/service accounts

## Ownership & Permissions

- `chown user file` → change owner
- `chown :group file` → change group
- `chown user:group file` → change owner & group
- `chmod 755 file` → rwxr-xr-x
- `chmod u+x file` → add execute to owner
- `chmod g-w file` → remove write from group
- Directory permissions:
  - `r` → list files
  - `w` → create/delete files
  - `x` → enter directory

## umask

- `umask` → check default permission mask
- `022` → files 644, dirs 755
- `077` → files 600, dirs 700
- umask removes permissions from defaults
## Users & Permissions

### User & Group Management

- useradd -m username
- groupadd devteam
- usermod -aG devteam username
- id username
- groups username

### Important Files

- /etc/passwd -> user account info
- /etc/shadow -> password hashes + password aging

### File Permissions

- chmod
- chown
- symbolic permissions:
  - chmod u+x file
  - chmod g-w file

- numeric permissions:
  - 7 = rwx
  - 6 = rw-
  - 5 = r-x
  - 4 = r--

### umask

- default file permissions -> 666
- default directory permissions -> 777

Example:
- umask 022
- files -> 644
- dirs -> 755

- umask removes permissions from default values

### Special Permissions

#### SUID

- chmod u+s file
- executes with file owner permissions

#### SGID

- chmod 2770 /project
- new files inherit parent directory group

#### Sticky Bit

- chmod +t /shared
- protects files in shared directories
- users cannot delete others' files
- commonly used on /tmp

### Shared Directory Practice

- created shared project directory
- group-based access control
- restricted outsider access
- tested permission denied scenarios

### ACL (Access Control Lists)

- setfacl
- getfacl

Example:
- setfacl -m u:qauser:rw project.txt

- allows user-specific permissions without changing ownership/group

### Process Handling

- ps aux | grep user
- pkill -u user
- userdel -r user

- learned why userdel fails when user processes are active
# 📘 Linux Learning Notes — 10 May 2026

## 🔹 Users & Groups

- Created users using:
  - `useradd`
  - `passwd`
- Created groups using:
  - `groupadd`
- Added users to groups:
  - `usermod -aG`

## 🔹 User Information Files

- `/etc/passwd`
  - stores user account details
  - readable by all users
- `/etc/shadow`
  - stores encrypted password hashes
  - accessible only by root

## 🔹 File Permissions

- Practiced:
  - `chmod`
  - `chown`
  - symbolic permissions
  - numeric permissions
- Observed Linux permission flow:
  - owner
  - group
  - others
  - ACL

## 🔹 SGID

- Applied SGID on shared directory:
  ```bash
  chmod 2770 /project
## Linux Notes - 11 May 2026

### File Permissions & Group Ownership
- Files created inside SGID directories inherit group ownership
- File permissions still depend on user umask
- `rw-r--r--` means:
  - owner = read/write
  - group = read only
  - others = read only
- Group members cannot edit file unless group write permission exists

### Useful Permission Commands
```bash
ls -l
ls -ld
chmod
chown
groups
id
namei -l <path>
```

### Important Permission Understanding
- File delete permission depends on directory permissions
- Directory write permission controls create/delete operations
- Execute permission on directory allows entering/accessing directory
- Read permission on directory allows listing contents

### SGID Notes
- SGID on directory makes new files inherit directory group ownership
- SGID does not automatically give group write permission

### SUID Notes
- SUID allows executable to run with file owner's privileges
- `/usr/bin/passwd` uses SUID so normal users can update passwords
- Password hashes are stored in `/etc/shadow`

### /etc/passwd vs /etc/shadow
- `/etc/passwd` stores:
  - username
  - UID
  - GID
  - home directory
  - shell
- `/etc/shadow` stores:
  - hashed passwords
  - password aging information

### Process & Job Control Revision
```bash
jobs
bg
fg
pgrep
kill
kill -9
top
htop
```

### Labs Practiced
- Shared directory with SGID
- Group ownership inheritance testing
- Permission denied troubleshooting
- SUID behavior understanding
- Process and background job handling

# 📘 Linux Notes — 12 May 2026

## 🔹 ACL (Access Control List)

### ACL Basics

- `setfacl -m` → modify ACL entries
- `getfacl <file/dir>` → view ACLs
- `+` in `ls -l` output means ACL exists

### ACL Entry Types

- `u:name:perm` → named user ACL
- `g:name:perm` → named group ACL
- `u::perm` → owner permissions
- `g::perm` → owning group permissions
- `m::perm` → ACL mask
- `o::perm` → others permissions

### ACL Mask

- Mask controls group-related permissions
- Mask affects:
  - named users
  - named groups
  - owning group
- Mask does NOT affect:
  - owner
  - others

### Effective Permissions

- ACL entry permissions can differ from effective permissions
- `#effective:` appears when mask restricts permissions

Example:

group::rwx
mask::r--
effective → r--

### Directory Permission Behavior

Directory permissions meaning:
- `r` → list filenames
- `w` → create/delete/rename
- `x` → enter/traverse directory

Behavior with only `r--` on directory:
- `ls /project` works
- `cd /project` fails
- cannot access inside files/directories

### SGID on Directories

- `chmod 2750 /project`
- SGID on directory makes new files inherit group ownership
- `rws` → SGID + execute
- `r-S` → SGID set but execute missing

### Shell / Prompt Learning

- `Ctrl + L` may fail in `sh/dash`
- `bash` supports PS1 formatting like:
  - `\u` → username
  - `\h` → hostname
  - `\w` → current directory

### Useful Commands

```bash
getfacl /project
setfacl -m u:qauser:r-x /project
setfacl -m m::r-- /project
groups ubuntu
echo $SHELL
ps -p $$

# Linux Notes - 13 May 2026

## Special Permissions
- Sticky bit:
  - `chmod 1777 dir`
  - only file owner/root/directory owner can delete files
  - does NOT control file content write access
- SGID on directory:
  - `chmod 2775 dir`
  - new files inherit directory group
- SUID:
  - `find /usr/bin -perm -4000`
  - program runs with owner privileges temporarily
  - `passwd` uses SUID root to update `/etc/shadow`

## passwd and shadow
- `/usr/bin/passwd` = program/tool
- `/etc/shadow` = password hash storage
- normal users cannot edit shadow directly
- passwd temporarily gets root effective UID via SUID

## Networking / Transfer
- Check port:
  - `nc -zv google.com 443`
- SCP download:
  - `scp -i key.pem user@host:/path/file .`
- SCP compression:
  - `scp -C`
- rsync compression:
  - `rsync -avz`

## tmux
- List sessions:
  - `tmux ls`
- Detach:
  - `Ctrl+b d`

## LVM
- Extend LV:
  - `lvextend -L +5G`
  - `resize2fs`
- Safe ext4 reduce order:
  - `umount`
  - `e2fsck -f`
  - `resize2fs`
  - `lvreduce`
  - `mount`
- XFS:
  - extend supported
  - shrink unsupported
- Snapshot:
  - `lvcreate -L 2G -s -n snapshot /dev/vg/lv`
  - `lvconvert --merge`
- Snapshot full:
  - becomes invalid/unusable

## Swap
- Create:
  - `fallocate -l 1G /swapfile`
- Enable:
  - `swapon /swapfile`
- Disable:
  - `swapoff /swapfile`
- Check:
  - `swapon --show`
  - `free -h`

## sed / grep / cut
- Replace first 10 lines:
  - `sed -i '1,10 s/INFO/LOG/g' sample.log`
- Print only first 10 modified lines:
  - `sed -n '1,10 { s/INFO/LOG/g; p }' sample.log`
- grep with line numbers:
  - `grep -n 'INFO' sample.log`
- cut extracts:
  - fields/columns

## Process Management
- Graceful kill:
  - `kill <PID>`
- Find PID:
  - `pgrep process_name`
- Background stopped job:
  - `bg`
- D state:
  - uninterruptible sleep (usually I/O wait)

## LVM Flow
- Disk -> PV -> VG -> LV -> Filesystem -> Mountpoint -> fstab
# Linux Notes - 14 May 2026

## Process Management & Debugging

- View processes:
  ps
  ps aux
  top
  htop

- PID = unique Process ID for running process

- TTY identifies terminal session:
  tty

- Find process:
  pgrep process_name
  ps aux | grep process_name

- Background process:
  sleep 300 &

- Show shell jobs:
  jobs

- Stop foreground process:
  Ctrl + Z

- Move stopped job to background:
  bg

- Bring process to foreground:
  fg

- Kill process gracefully:
  kill PID
  Default signal = SIGTERM (15)

- Force kill:
  kill -9 PID
  Signal = SIGKILL (9)

- Zombie process:
  Finished execution but parent has not collected exit status using wait()

- D state:
  Uninterruptible sleep usually waiting for I/O operation

- Create CPU load:
  yes > /dev/null &

- Monitor high CPU:
  top
  ps aux --sort=-%cpu

- Nice value:
  Higher nice = lower priority

- Change priority:
  renice 10 -p PID

- top shortcuts:
  Shift + P -> sort by CPU
  Shift + M -> sort by memory
  d -> change refresh interval
  q -> quit


## Process Management & Disk Troubleshooting

### Process Management
- Used:
  ```bash
  ps
  ps aux
  pgrep
  top
  htop
  ```
- Practiced background and foreground jobs:
  ```bash
  jobs
  bg
  fg
  ```
- Created background processes:
  ```bash
  sleep 1000 &
  ```
- Learned process states:
  - `R` = Running
  - `S` = Sleeping
  - `T` = Stopped
  - `Z` = Zombie
  - `D` = Uninterruptible sleep
- Observed zombie process check:
  ```bash
  ps -el | grep ' Z '
  ```
- Learned:
  - `kill PID` → graceful termination using SIGTERM(15)
  - `kill -9 PID` → force kill using SIGKILL(9)
- Created CPU load:
  ```bash
  yes > /dev/null &
  ```
- Sorted processes:
  ```bash
  ps aux --sort=-%cpu
  ```
- Learned difference:
  - `yes /dev/null &` → prints `/dev/null`
  - `yes > /dev/null &` → discards output silently
- Learned nice/renice:
  ```bash
  nice -n 10 command
  renice 5 -p PID
  ```
- Learned:
  - higher nice value = lower priority
  - lower nice value = higher priority

### Disk & Storage Troubleshooting
- Checked filesystem usage:
  ```bash
  df -h
  ```
- Checked inode usage:
  ```bash
  df -i
  ```
- Found large directories/files:
  ```bash
  du -sh *
  du -ah . | sort -rh | head
  ```
- Learned difference:
  - `df` → filesystem usage
  - `du` → file/directory usage
- Simulated disk usage:
  ```bash
  fallocate -l 100M file1
  ```
- Explored mounted filesystems:
  ```bash
  mount
  lsblk
  ```
- Explored persistent mounts:
  ```bash
  cat /etc/fstab
  ```
- Learned:
  ```bash
  mount -a
  ```
  tests all mounts from `/etc/fstab`
# linux_notes.md

## Disk Usage & Troubleshooting

### df vs du
- `df -h` -> filesystem level disk usage
- `du -sh *` -> directory/file usage
- `df` checks filesystem/block device usage
- `du` checks actual file/directory consumption

### Useful du Commands

```bash
du -sh *
du -ah . | sort -rh | head
du -ah / | sort -rh | head -20
```

- `-s` -> summary only
- `-h` -> human readable
- `-a` -> include files
- `sort -rh` -> reverse human readable sorting

### Disk Full Troubleshooting Flow

```bash
df -h
du -sh /*
du -ah / | sort -rh | head -20
rm unwanted_file
df -h
```

### fallocate

Creates large files quickly.

```bash
fallocate -l 500M bigfile
```

### lsblk Understanding

- `disk` -> physical/virtual disk
- `part` -> partition
- `loop` -> kernel-created virtual block devices

### fstab Concepts

- `/etc/fstab` used for persistent mounts
- UUID preferred over `/dev/sdb1`

Verify fstab safely:

```bash
umount /data
mount -a
```

Example:

```bash
UUID=<uuid> /data ext4 defaults,nofail 0 2
```

### mount -a

Mounts all filesystems from `/etc/fstab`.

### Swap Commands

```bash
swapon --show
free -h
sysctl vm.swappiness
cat /proc/sys/vm/swappiness
```

### Create Swap File

```bash
dd if=/dev/zero of=/swapfile bs=1M count=1024
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```

### Process Management

```bash
ps aux
kill -9 PID
bg
fg
```

### LVM Flow

```text
Disk/Partition -> PV -> VG -> LV -> Filesystem -> Mount Point -> fstab
```

### Safe LV Reduce Flow

```bash
umount
e2fsck
resize2fs
lvreduce
mount
```

- filesystem must shrink before LV reduce
- reducing incorrectly can corrupt data

### SGID

- SGID on directory inherits parent group ownership
- useful for shared directories

### User & Group Concepts

- `/etc/passwd` -> user account information
- `/etc/shadow` -> password hashes + aging/security info
- `/etc/group` -> group information

### UID Ranges

- UID 0 -> root
- UID 1-999 -> system/service accounts
- UID >=1000 -> normal users

### User Management

```bash
useradd -m username
passwd username
groupadd groupname
usermod -aG groupname username
id
```

### Lock User

```bash
passwd -l username
usermod -L username
``
### SUID vs SGID

- SUID -> execute with file owner privileges
- SGID -> execute/inherit with file group privileges

### Loop Device Lab

```bash
losetup -fP /root/lvm_disk.img
```

- attaches image file as virtual block device
- useful for LVM/filesystem labs
 

## LVM & Snapshot Practical Notes

### Create Virtual Disk for LVM
- `sudo dd if=/dev/zero of=/root/lvm_disk.img bs=1M count=2048`
- creates 2GB virtual disk image file

### Attach Virtual Disk as Loop Device
- `sudo losetup -fP /root/lvm_disk.img`
- `-f` -> find first free loop device
- `-P` -> scan partitions
- loop devices behave like virtual disks backed by files
- snap packages also use loop devices

### Verify Loop Devices
- `losetup -a`
- `lsblk`

### LVM Flow
Disk/Loop Device -> PV -> VG -> LV -> Filesystem -> Mount Point

### Create LVM
- `sudo pvcreate /dev/loop4`
- `sudo vgcreate data-vg /dev/loop4`
- `sudo lvcreate -L 2G -n data-lv data-vg`

### Create Filesystem & Mount
- `sudo mkfs.ext4 /dev/data-vg/data-lv`
- `sudo mkdir /data`
- `sudo mount /dev/data-vg/data-lv /data`

### Important LVM Commands
- `sudo pvs`
- `sudo vgs`
- `sudo lvs`
- `lsblk`

### LVM Attribute Meanings
- `o` -> origin LV
- `O` -> origin LV under merge
- `s` -> snapshot
- `w` -> writable
- `a` -> active
- `I` -> invalid snapshot
- `i` -> inherited allocation policy

### Ext4 LVM Reduction Workflow
Correct order:
- umount -> e2fsck -> resize2fs -> lvreduce -> mount

Commands:
- `sudo umount /data`
- `sudo e2fsck -f /dev/data-vg/data-lv`
- `sudo resize2fs /dev/data-vg/data-lv 1536M`
- `sudo lvreduce -L 1536M /dev/data-vg/data-lv`

Important:
- reduce filesystem first before reducing LV
- reducing LV first can corrupt filesystem

### Snapshot Creation
- `sudo lvcreate -L 300M -s -n snap-lv /dev/data-vg/data-lv`

### Mount Snapshot Read-Only
- `sudo mount -o ro /dev/data-vg/snap-lv /snap`

### Snapshot Concepts
- snapshots use Copy-On-Write (COW)
- snapshot stores changed blocks only
- deleting file does not immediately increase snapshot size
- overwriting/reusing blocks increases snapshot Data%
- snapshot preserves point-in-time filesystem state

### Snapshot Overflow
- small snapshot + heavy writes -> invalid snapshot
- `I` in lvs output means invalid snapshot

### Snapshot Merge
- `sudo lvconvert --merge /dev/data-vg/snap-lv`
- restores origin LV to snapshot state
- newer data after snapshot creation is lost after rollback

### Activate/Deactivate LV
- `sudo lvchange -an /dev/data-vg/data-lv`
- `sudo lvchange -ay /dev/data-vg/data-lv`

Important:
- `-an` -> deactivate LV
- `-ay` -> activate LV
- inactive LV cannot be mounted

### Cleanup LVM Lab
- `sudo umount /data`
- `sudo lvremove /dev/data-vg/data-lv`
- `sudo vgremove data-vg`
- `sudo pvremove /dev/loop4`
- `sudo losetup -d /dev/loop4`
- `sudo rm -f /root/lvm_disk.img`

---

## Linux Permissions Notes

### Sticky Bit
- `drwxrwxrwt`
- only owner/root can delete respective files
- common example: `/tmp`

### SGID
- `drwxrws---`
- new files inherit parent directory group ownership

### SUID
- `-rwsr-xr-x`
- executable runs with owner privileges
- example: `passwd`

### ACL Commands
- `sudo setfacl -m u:user:rx /project`
- `sudo getfacl /project`

Important:
- ACL provides user-specific permissions without changing groups

### Umask Concepts
- `666 - 022 = 644`
- `666 - 002 = 664`
- `777 - 022 = 755`

Important:
- umask removes permissions from defaults
- `0002` useful for collaborative environments

---

## Process Management Notes

### Process Commands
- `ps`
- `ps aux`
- `jobs`
- `tty`

### Process Concepts
- PID -> unique process identifier
- foreground process blocks terminal
- background process does not block terminal

### Background Jobs
- `sleep 100 &`
- `&` runs process in background

### Suspend Process
- `Ctrl + Z`
- sends SIGTSTP
- temporarily suspends foreground process

### Resume Jobs
- `fg`
- `bg`

### jobs Command
- shows shell background/suspended jobs only
/



## Logs & Debugging Notes

### Important Log Locations

| File/Directory | Purpose |
|---|---|
| `/var/log/syslog` | General system logs |
| `/var/log/auth.log` | Authentication, sudo, SSH logs |
| `/var/log/kern.log` | Kernel logs |
| `/var/log/dpkg.log` | Package installation logs |
| `/var/log/journal/` | systemd journal storage |

### Traditional Logging vs systemd Journal

Traditional logging:
- Separate plain text log files
- Managed by rsyslog/syslog
- Logs categorized into different files

Examples:
- auth.log
- syslog
- kern.log

Modern systemd journal:
- Centralized logging system
- Managed by `systemd-journald`
- Viewed using `journalctl`
- Supports filtering by:
  - service
  - priority
  - boot
  - time

### Important journalctl Commands

```bash
journalctl
```

View all logs

```bash
journalctl -n 20
```

- `-n` = show recent lines

```bash
journalctl -f
```

- `-f` = follow logs live

```bash
journalctl -u ssh
```

- `-u` = filter by service/unit

```bash
journalctl -b
```

- `-b` = logs from current boot

```bash
journalctl -b -1
```

- `-1` = previous boot

```bash
journalctl -p err
```

- `-p` = priority/severity filter
- `err` = show error level logs

```bash
journalctl -u ssh --no-pager
```

- `--no-pager` = print directly to terminal without opening less

### Log Severity Levels

| Level | Meaning |
|---|---|
| emerg | system unusable |
| alert | immediate action required |
| crit | critical issue |
| err | error |
| warning | warning |
| notice | important normal event |
| info | informational |
| debug | debugging details |

### logger Command

```bash
logger "message"
```

Used to manually generate custom log entries into:
- syslog
- journalctl

Examples practiced:

```bash
logger "This is test log from ram"
logger "this is hello from ubuntu"
```

### less Search Practice

Inside `less`:
- `/word` = search
- `n` = next match
- `N` = previous match
- `Shift + G` = go bottom
- `g` = go top
- `q` = quit

### systemd Notes

```bash
ps -p 1
```

Verified:
- PID 1 = systemd

systemd responsibilities:
- service management
- boot process
- logging
- process supervision

### SSH Log Understanding

Observed successful SSH login logs:
- `Accepted publickey`
- `pam_unix(sshd:session)`
- source IP address
- SSH key authentication

### Important Troubleshooting Workflow

```text
Issue
→ systemctl status service
→ journalctl -u service
→ identify actual error
→ fix issue
→ restart service
→ verify
```

## 2026-05-19

### systemd & Services Debugging
- Used `systemctl list-units --type=service --state=running` to inspect running services
- Learned difference between:
  - `systemctl` → service management/status
  - `journalctl` → log inspection
- Checked SSH service state using:
  - `systemctl status ssh`
  - `journalctl -u ssh`
- Practiced:
  - `start`
  - `stop`
  - `restart`
  - `enable`
  - `disable`
- Understood:
  - `active (running)`
  - `inactive (dead)`
  - `failed (exit-code)`
- Learned service uptime interpretation from `systemctl status`
- Observed SSH daemon logs:
  - successful logins
  - invalid users
  - authentication failures
  - bot SSH scans on public EC2

### Service Failure Debugging
- Intentionally broke SSH config in `/etc/ssh/sshd_config`
- Observed failed restart behavior:
  - restart loops
  - `Start request repeated too quickly`
  - `status=255/EXCEPTION`
- Used:
  - `journalctl -xeu ssh.service`
  - `sudo sshd -t`
- Identified exact parsing/configuration error:
  - `Bad configuration option: INVALIDCONFIG`
- Learned:
  - systemd logs show service lifecycle/errors
  - application validation tools reveal exact root cause

### SSH Recovery Workflow
- Fixed invalid SSH config
- Solved runtime error:
  - `Missing privilege separation directory: /run/sshd`
- Created required runtime directory:
  - `sudo mkdir -p /run/sshd`
  - `sudo chmod 755 /run/sshd`
- Verified config using:
  - `sudo sshd -t`
- Restarted and validated SSH recovery successfully

### Logs & Debugging
- Practiced:
  - `grep -Ei`
  - `journalctl -n`
  - `tail`
- Learned alias behavior:
  - `alias grep='grep --color=auto'`
- Understood pager navigation in `less`:
  - `q`
  - `g`
  - `G`
  - `/search`
  - `n`

### Disk Space Troubleshooting
- Simulated low disk space using:
  - `fallocate -l`
- Verified usage using:
  - `df -h`
- Investigated large directories/files using:
  - `du -sh /* | sort -hr`
  - `find / -type f -size +500M`
- Learned:
  - `df` = filesystem overview
  - `du` = directory/file usage
- Understood recursive disk usage investigation workflow

### Process & Job Control
- Practiced:
  - foreground/background jobs
  - `Ctrl+C`
  - `Ctrl+Z`
  - `jobs`
  - `bg`
  - `fg`
  - `kill`
  - `pgrep`
- Learned signals:
  - `SIGINT`
  - `SIGTERM`
  - `SIGTSTP`

### Package Management
- Used:
  - `apt show nginx`
  - `apt list --installed`
  - `dpkg -l`
- Learned:
  - package metadata
  - dependencies
  - repository sources
  - installed vs available packages

### Shell Scripting & Permissions
- Created and executed shell scripts
- Learned difference between:
  - `./script.sh`
  - `sh script.sh`
- Understood execute permission behavior:
  - execute bit required for direct execution
  - read permission enough when interpreter reads file

### Vim
- Learned:
  - `:w`
  - `:q`
  - `:wq`
  - `:q!`
  - `:wq!`
- Understood `!` means force/override warnings

## Package Management & Recovery Notes

### Package Search
- `apt search nginx`
  - searches repository metadata for matching package names/descriptions
- `apt search ^nginx$`
  - `^` = start
  - `$` = end
  - exact package match only

### Verify Installed Packages
- `dpkg -l`
  - list package states
- `ii`
  - package installed + configured
- `rc`
  - package removed but config files remain

### Package Information
- `dpkg -s git`
  - detailed package metadata
- shows:
  - version
  - dependencies
  - maintainer
  - description
  - install status

### Dependencies
- `apt depends nginx`
  - inspect dependency tree
- important fields:
  - `Depends`
  - `Breaks`
  - `Replaces`

### Package Lifecycle
- `apt install`
  - installs package + dependencies
- `apt remove`
  - removes binaries but may keep configs
- `apt purge`
  - removes binaries + configs
- `apt autoremove`
  - removes orphaned auto-installed dependencies

### Package States Learned
- `ii`
  - installed/configured
- `rc`
  - removed/config files remain
- no output from `dpkg -l`
  - package fully purged

### Update vs Upgrade
- `apt update`
  - refresh repository/package metadata only
- `apt upgrade`
  - actually upgrades installed packages
- stale metadata can prevent latest upgrades

### Package Recovery Commands
- `apt install -f`
  - fix broken dependency/package states
- `dpkg --configure -a`
  - configure unfinished/unconfigured packages

### Package Manager Lock Troubleshooting
- only one apt/dpkg process allowed at a time
- lock issue debug:
  - `ps aux | grep apt`
- process states observed:
  - `T`
  - `T+`
- graceful terminate:
  - `kill PID`
- force terminate:
  - `kill -9 PID`

### dpkg Useful Commands
- `dpkg -L apache2`
  - list files installed by package
- `dpkg -S /usr/sbin/apache2`
  - find package owning file

### Important Linux File Locations
- `/etc`
  - configs
- `/var/log`
  - logs
- `/usr/sbin`
  - admin binaries
- `/usr/lib/systemd/system`
  - service files
- `/var/www/html`
  - web root

### Service Integration
- package installation can automatically:
  - start services
  - enable services
  - install systemd unit files

### Apache Package Split
- `apache2`
  - configs/service integration
- `apache2-bin`
  - binaries/modules
- `apache2-data`
  - static files/icons/errors
- `apache2-utils`
  - utility tools

### Real Troubleshooting Mindset
- inspect actual errors first
- never delete dpkg lock files manually
- identify locking process properly
- verify package state after recovery
- understand package lifecycle practically


## Linux Networking Basics

### IP & Interfaces
- `ip a`
  - shows interfaces, IP addresses, interface state
  - `lo` = loopback interface
  - `ens5` = EC2 network interface

- `ip -4 a`
  - show IPv4 addresses only

- `hostname -I`
  - quickly display system IP addresses

### Port & Service Inspection
- `ss -tulnp`
  - `-t` = TCP
  - `-u` = UDP
  - `-l` = listening sockets
  - `-n` = numeric ports
  - `-p` = process using port

- `ss -tulnp | grep :80`
  - filter specific listening port

- `netstat -tulnp`
  - legacy networking inspection tool
  - `ss` is modern replacement

### Connectivity Testing
- `ping 8.8.8.8`
  - tests network connectivity

- `ping google.com`
  - tests connectivity + DNS resolution

- if IP ping works but domain ping fails:
  - likely DNS issue

### curl
- `curl https://example.com`
  - fetch webpage content

- `curl -I https://example.com`
  - fetch HTTP headers only

- `curl -v https://example.com`
  - verbose HTTP/TLS/debugging output

- `curl localhost`
  - test local service without browser

### wget
- `wget https://example.com`
  - download file from URL

### Service → Process → Port Flow
- nginx service starts nginx process
- nginx process listens on port 80
- `ss -tulnp` verifies listening port
- `curl localhost` verifies HTTP response

### Package Management Understanding
- `apt` = high-level package manager
- `dpkg` = low-level package installer/backend

- `history.log`
  - high-level apt command history

- `dpkg.log`
  - detailed package state transitions

### apt Concepts
- manual package:
  - protected from autoremove
  - not necessarily manually installed by human

- automatic package:
  - dependency-installed package
  - removable if no package depends on it

### Version Inspection
- `apt policy apache2`
  - Installed = current installed version
  - Candidate = version apt wants to install
  - Version table = available repository versions

- Ubuntu repositories:
  - `resolute` = base release
  - `resolute-updates` = bug fixes
  - `resolute-security` = security patches

## Linux Networking Basics Lab

- inspected interfaces using:
  ```bash
  ip a
  ip -4 a
  hostname -I
  ```

- identified:
  - `lo`
    - localhost/loopback interface
  - `ens5`
    - EC2 primary network interface

- learned:
  - `UP`
    - interface enabled
  - `LOWER_UP`
    - network link working
  - `inet`
    - IPv4 address
  - `127.0.0.1`
    - localhost/self communication
  - EC2 internally uses private VPC IPs (`172.31.x.x`)

- understood difference:
  - `hostname -I`
    - quick IP display
  - `ip a`
    - detailed interface/network inspection

### Port & Service Inspection

- used:
  ```bash
  ss -tulnp
  sudo ss -tulnp
  netstat -tulnp
  sudo netstat -tulnp
  ```

- learned:
  - `ss`
    - modern socket inspection tool
  - `netstat`
    - older legacy networking tool

- understood flags:
  - `-t`
    - TCP
  - `-u`
    - UDP
  - `-l`
    - listening sockets
  - `-n`
    - numeric output
  - `-p`
    - process info

- observed:
  - SSH listening on port `22`
  - Apache listening on port `80`
  - DNS resolver on port `53`
  - DHCP client on port `68`

- understood:
  - `LISTEN`
    - waiting for incoming connections
  - `0.0.0.0:PORT`
    - listening on all IPv4 interfaces
  - `[::]:PORT`
    - listening on all IPv6 interfaces
  - `127.0.0.1:PORT`
    - localhost-only access

- understood relationship:
  ```text
  process → socket → protocol → port → client access
  ```

### Package Verification

- verified package installation using:
  ```bash
  dpkg -l | grep net-tools
  dpkg -s net-tools
  ```

- learned:
  - `dpkg -l`
    - list installed packages
  - `dpkg -s`
    - check package status/details

### Networking Concepts

- reviewed common ports:
  - `22/tcp`
    - SSH
  - `80/tcp`
    - HTTP
  - `53`
    - DNS
  - `68/udp`
    - DHCP client

- reviewed:
  - ICMP
    - Internet Control Message Protocol

### Ping & Connectivity Testing

- practiced:
  ```bash
  ping google.com
  ping 8.8.8.8
  ```

- learned:
  - `ping IP`
    - tests network connectivity only
  - `ping domain`
    - tests connectivity + DNS resolution

- understood:
  - `icmp_seq`
    - packet sequence number
  - `ttl`
    - Time To Live
  - `time=ms`
    - latency/round-trip time

- practiced process control:
  - `Ctrl + C`
    - terminate process (`SIGINT`)
  - `Ctrl + Z`
    - suspend process (`SIGTSTP`)

### curl & HTTP Debugging

- practiced:
  ```bash
  curl https://google.com
  curl -I https://google.com
  curl -v https://google.com
  curl -I localhost
  ```

- learned:
  - `curl`
    - sends HTTP/HTTPS requests from terminal
  - `-I`
    - fetch headers only
  - `-v`
    - verbose/debug mode
  - `-o`
    - save output to file

- understood verbose stages:
  ```text
  DNS resolution
  → TCP connection
  → TLS handshake
  → certificate validation
  → HTTP request
  → HTTP response
  ```

- observed:
  - HTTP `200 OK`
  - HTTP `301 Moved`
  - HTTP/2 negotiation
  - TLSv1.3 connection
  - SSL certificate verification

- verified:
  - working DNS
  - outbound internet connectivity
  - HTTPS functionality
  - Apache responding locally on `localhost`

## Linux Networking & Web Server Practical

### wget vs curl
- `wget URL`
  - downloads file locally
  - saves automatically
- `curl URL`
  - prints response directly to terminal
- `curl -I`
  - fetch headers only
- `curl -X METHOD URL`
  - specify HTTP request method manually

### HTTP Testing
- `curl localhost`
- `curl 127.0.0.1`
- localhost resolves to `127.0.0.1`
- curl acts like lightweight browser for testing HTTP/services

### Networking Concepts
- Public IP vs Private IP vs localhost
- `127.0.0.1`
  - loopback/local machine only
- `172.x.x.x`
  - private/internal network IP
- Public IP used for internet access

### Ports & Sockets
- socket = communication endpoint created by process
- port = numeric identifier for service communication
- process creates socket and binds to port

### ss Command
- `ss -tulnp`
  - `-t` TCP sockets
  - `-u` UDP sockets
  - `-l` listening sockets only
  - `-n` numeric IPs/ports
  - `-p` process using socket

### nginx Practical
- installed nginx using apt
- verified service with:
  - `systemctl status nginx`
- verified listener:
  - `ss -tulnp | grep :80`
- nginx listening on port 80

### Real Troubleshooting
- nginx failed due to:
  - `bind() to 0.0.0.0:80 failed`
- identified Apache using port 80
- checked process using port:
  - `ss -tulnp | grep :80`
  - `lsof -i :80`
- stopped apache2
- restarted nginx successfully

### Parent/Worker Process Understanding
- master/root process starts first
- worker processes run under `www-data`
- checked with:
  - `ps -o pid,ppid,user,cmd -C apache2`

### AWS External Access
- identified public IP from AWS console
- configured security group inbound HTTP rule
- allowed TCP port 80
- tested external browser access successfully

### Service Failure Debugging
- stopped nginx intentionally
- verified:
  - inactive service
  - no listener on port 80
  - curl connection failure
- restarted nginx and restored HTTP access

### Important Troubleshooting Chain
service
↓
process
↓
socket
↓
port
↓
network access
↓
client request
↓
response/debugging

## Linux Networking & Troubleshooting Labs

### Networking Basics

- Practiced checking IP addresses and network interfaces using:
  - `ip a`
- Practiced checking listening ports using:
  - `ss -tulnp`
- Learned practical port verification and service inspection
- Practiced connectivity testing using:
  - `ping`
  - `curl`
  - `wget`
- Tested local HTTP access using:
  - `curl localhost`
  - `curl 127.0.0.1`
- Practiced verifying services through:
  - service status
  - listening ports
  - local connectivity testing

### High CPU Troubleshooting

- Simulated CPU load using:
  - `yes > /dev/null`
- Monitored CPU usage using:
  - `top`
  - `ps aux --sort=-%cpu | head`
- Identified high CPU process using PID
- Learned process states:
  - `R` → running
  - `T` → stopped/suspended
- Practiced stopping runaway processes using:
  - `kill`
  - `pkill`
- Learned operational troubleshooting workflow for CPU issues

### Disk & Filesystem Troubleshooting

- Practiced disk usage inspection using:
  - `df -h`
- Practiced inode inspection using:
  - `df -i`
- Learned difference between:
  - disk space exhaustion
  - inode exhaustion
- Created large files for disk usage testing
- Practiced identifying large disk usage locations

### Service Troubleshooting

- Practiced service management using:
  - `systemctl status`
  - `systemctl start`
  - `systemctl stop`
- Verified listening ports after service changes
- Learned service → process → port troubleshooting flow
- Practiced checking logs using:
  - `journalctl -xe`
  - `journalctl -u nginx`

### Linux Permissions Troubleshooting

- Practiced executable permission debugging using:
  - `chmod 644`
  - `chmod +x`
- Learned why scripts fail with:
  - `Permission denied`
- Practiced verifying permissions using:
  - `ls -l`
- Learned executable bit importance in script execution

### Shell & Execution Understanding

- Learned difference between:
  - `sh script.sh`
  - `bash script.sh`
  - `./script.sh`
- Understood interpreter execution vs executable permission execution
- Practiced Linux command execution behavior and troubleshooting

### Linux File Operations

- Practiced:
  - `ls -lt`
  - `ls -ltr`
- Learned file sorting by modification time
- Practiced file inspection using:
  - `cat`
  - `vim`

### Linux Troubleshooting Mindset

- Practiced real troubleshooting workflow:
  - read actual error
  - verify service status
  - verify process
  - verify listening port
  - test locally
  - test externally
  - inspect logs
  - retest after fix

## Commands Practiced

```bash
ip a
ss -tulnp
ping
curl
wget
top
ps aux --sort=-%cpu | head
kill
pkill
df -h
df -i
systemctl status
systemctl start
systemctl stop
journalctl -xe
journalctl -u nginx
chmod 644
chmod +x
ls -lt
ls -ltr
cat
vim

# Tmux + Shell Environment Notes

## Tmux Colors / Prompt
- Customized Bash prompt using `PS1`
- Used ANSI escape color codes inside `.bashrc`
- Green username/hostname prompt example:
  export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

## Bash Startup Files
- `.bashrc` loads for interactive shells
- `.profile` may not exist on some minimal/cloud Ubuntu images
- Added Bash config manually to ensure prompt loads automatically after login

## EC2 / Ubuntu Notes
- Verified shell customization on EC2 instance
- Learned why changes may not apply until:
  - new shell session
  - `source ~/.bashrc`
  - proper startup file exists

## Git / Project Notes
- Moving local folders changes Git tracking path locally
- GitHub repository structure changes only after commit + push
- Missing one daily commit is normal — continue consistency next day

## Windows Notes
- Direct shutdown is generally safe on Windows 11
- Unsaved work or running updates can still cause issues

## Linux Notes

### WSL Workflow
- Moved development workflow from Windows path to native WSL projects directory
- Cloned GitHub repositories directly inside WSL
- Learned Windows path to WSL path conversion:
  - `D:\path` → `/mnt/d/path`

### Aliases
- Created custom aliases:
  - `projects='cd ~/projects'`
- Viewed aliases using:
  - `alias`
- Learned aliases should be stored in:
  - `~/.bash_aliases`
  - loaded from `~/.bashrc`

### tmux
- Learned tmux keeps persistent shell sessions
- Reloaded aliases using:
  - `source ~/.bashrc`
- Learned tmux sessions preserve old shell environment

### Permissions
- Practiced file permissions using:
  - `chmod +x`
  - `chmod u-r`
- Learned difference between:
  - read permission
  - execute permission

### Bash Conditions
- `-r` → checks file readability
- `-z` → checks if string is empty

### Disk Usage Commands
- `df -h /`
  - shows filesystem disk usage
- `du -sh /`
  - shows directory/file usage recursively

### Important Difference
- `df` checks filesystem usage
- `du` checks actual file/directory sizes

### sort Flags
- `sort -rh`
  - `-r` → reverse sort
  - `-h` → human-readable numeric sorting

### Process & Signals
- `kill -9`
  - sends SIGKILL signal
- Learned difference between:
  - SIGTERM
  - SIGKILL

### nice / renice
- Practiced process priority concepts
- Learned:
  - `nice -n`
  - `renice`
- Understood nice value ranges:
  - `-20` high priority
  - `19` low priority
