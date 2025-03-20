---
title: "How to Clear APT Cache and Free Up Disk Space"
date: 2025-01-29
---

The APT cache stores downloaded package files on your system. Over time, this cache can take up significant disk space. How do you clear the apt cache? You simply use this apt clean command option:

```bash

apt clean
```

Clearing the APT cache is a simple way to free up space while keeping your system running smoothly.

## Clear Apache Cache: Step by Step Guide

### Step 1: Check Disk Space Usage

Before clearing the APT cache, check how much disk space is currently used by the cache.

Open a terminal and execute the following command:

```
sudo du -sh /var/cache/apt
```

This will show the size of the APT cache folder.

### Step 2: Clear the APT Cache

Now, use the following command to clear the APT cache from your Debian system

- **Clear all cached files:** Run this command to remove all files in the cache:
    
    ```
    sudo apt clean
    ```
    
    This will delete all downloaded package files, freeing up disk space.
    
- **Remove unused package files:** To remove unnecessary files left by incomplete or failed installations, use:
    
    ```
    sudo apt autoclean
    ```
    
    This only removes files that are no longer needed.
    

### Step 3: Remove Unused Packages

Sometimes, old or unused packages take up space. To remove them, use:

```
sudo apt autoremove
```

This will uninstall packages that were automatically installed as dependencies but are no longer needed.

### Step 4: Verify Disk Space

After clearing the cache and removing unused packages, check your disk space again to confirm:

```
sudo du -sh /var/cache/apt
```

You should see that the size of the cache folder is much smaller.

## Conclusion

It is safe to clear the APT cache and free up disk space on your Linux system. Use the `apt clean` and `apt autoclean` commands regularly to keep your system efficient. If you no longer need unused packages, `apt autoremove` can help to remove them from your system. These steps ensure your system has more available space and runs smoothly.

The post How to Clear APT Cache and Free Up Disk Space appeared first on TecAdmin.

Go to Source
