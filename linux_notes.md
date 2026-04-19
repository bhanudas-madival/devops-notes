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


