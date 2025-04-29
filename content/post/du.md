---
title: "du command"
date: 2025-04-16
draft: false
type: posts
categories:
  - du, linux
tags:
  - linux, du
summary: "Working with du command "
---


Working with ``du`` command in linux efficiently
---

## **1. Find the Largest Directories (Sorted)**
```bash
sudo du -ahx / | sort -rh | head -20
```
- `-a` → Show both **files** and **directories**  
- `-h` → Human-readable sizes (e.g., MB, GB)  
- `-x` → Stay on the same filesystem (avoid mounted drives)  
- `sort -rh` → Sort by **size**, largest first  
- `head -20` → Show **top 20** largest directories/files  

---

## **2. Find the Largest Directories Only (Excluding Files)**
```bash
sudo du -hx --max-depth=3 / | sort -rh | head -20
```
- `--max-depth=3` → Limits output to top 3 levels for better readability

---

## **3. Find the Largest Files (Over 500MB)**
```bash
sudo find / -type f -size +500M -exec du -h {} + | sort -rh | head -20
```
- `-type f` → Only files  
- `-size +500M` → Files larger than **500MB**  
- `du -h` → Show **file sizes** in human-readable format  

---

## **4. Exclude Certain Directories (Like /proc, /sys, etc.)**
```bash
sudo du -ahx --exclude={/proc,/sys,/dev,/run,/snap,/tmp,/mnt,/media} / | sort -rh | head -20
```
- This avoids system directories that don’t consume real disk space.

---

## **5. Find the Largest Users (Disk Usage by User)**
```bash
sudo du -sh /home/* 2>/dev/null
```
- This shows how much each user is consuming in `/home`.

---

## **6. Save Output to a File**
If you want to analyze later:
```bash
sudo du -ahx / | sort -rh > large_files.txt
```
Then open it:
```bash
less large_files.txt
```

---

### **Next Steps After Finding Large Files:**
1. **Check logs**: 
   ```bash
   sudo du -sh /var/log/*
   ```
   - You can clear logs with:  
     ```bash
     sudo journalctl --vacuum-time=7d  # Keep logs for 7 days
     ```
   
2. **Check package cache**:
   ```bash
   sudo du -sh /var/cache/apt
   ```
   - Clean it with:
     ```bash
     sudo apt clean
     ```

3. **Check old kernels**:
   ```bash
   dpkg --list | grep linux-image
   ```
   - Remove old ones (except the current):
     ```bash
     sudo apt remove --purge linux-image-OLD-VERSION
     ```

---