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
