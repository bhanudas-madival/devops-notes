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
