---
title: "How to Restore Deleted Files Under /etc Directory in Ubuntu"
date: 2025-03-19
---

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/disk-icon-250x250.webp)

Messed up or mistakenly deleted configuration files under `/etc` directory, but no backup? Here’s how to restore the files just like they were originally installed.

In Linux, the `/etc` directory is the location for storing system-wide configuration files. When installing a software package, it may create and/or read software specific config files under `/etc` directory for user custom configurations.

To prevent overriding user custom configurations, re-install software packages by default will NOT replace the files under `/etc` directory, even when you deleted the files under that directory. Though, some packages will ask if keep or replace the old configurations during installation.

If you mistakenly deleted files under `/etc` or just want to restore files under that directory to original, then this tutorial may help.

**NOTE: NOT only for Ubuntu, this tutorial should works in all Debian based systems.**

### Step 1: Find out which package includes the file/files

In Linux, many applications are built into separated packages. When installing software package `xxx`, it may also install `xxx-data`, `xxx-common`, or `xxx-plugins` etc. as dependencies. The file you want to restore may be included in either one of the packages.

#### Option 1: use dpkg command:

To find out which package includes that file, either open terminal (**Ctrl+Alt+T**) or connect to Debian/Ubuntu server. Then, try `dpkg -S` command.

For example, search which package includes `/etc/ssh/ssh_import_id`:

```
dpkg -S /etc/ssh/ssh_import_id
```

Or, you may search which package includes files under given folder (it MAY output multiple packages for single folder).

```
dpkg -S /etc/ssh
```

If you don’t even remember the file path, then just type filename instead. Even for file-name, you may use asterisk (\*) for the part that you don’t remember.

```
dpkg -S ssh_imp*
```

The last command will search which package include the file with `ssh_imp` at the beginning of its name.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/dpkg-query-search-700x475.webp)

Tips: you may alternatively use `apt-file search` command do the similar job, though it seems that it does not support asterisk in search text.

#### Option 2: use ucfq command:

The `dpkg -S` command sometimes does not work, because the packages you search may be handled by `ucf`（Update Configuration Files).

In the case, you may use `ucfq` instead. For example, search which package include `/etc/ssh/sshd_config`:

```
ucfq /etc/ssh/sshd_config
```

The command needs to input full-path to the file you want to search. If you don’t remember, try command below to instead (replace `smb` according to what you want to search):

```
grep smb /var/lib/ucf/registry
```

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/ucfq-search-700x313.webp)

### Step 2: Re-install package & specify to re-create non-exist files

After finding out the which package includes the file you want to restore, then, run the command below to re-install and re-generate the missing user configuration file:

```
sudo UCF_FORCE_CONFFMISS=1 apt -o Dpkg::Options::=--force-confmiss install --reinstall openssh-server
```

In the command above, replace the package name `openssh-server` accordingly.

Here both `UCF_FORCE_CONFFMISS=1` and `-o Dpkg::Options::=--force-confmiss` tell to re-generate installation files if they are NOT exist. You may skip the former one, if the package is NOT handled by `ucf`.

In the output, you’ll see something look like “Recreating deleted config file … with new version, as asked”.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/reinstall-override-700x348.webp)

If it’s a local `.deb` package, and you want to use `dpkg` command to install, then run the command below will do the similar things:

```
sudo UCF_FORCE_CONFFMISS=1 dpkg -i --force-confmiss package-name.deb
```

According to ucf manpage, there are also `UCF_FORCE_CONFFNEW` and `--force-confnew` dpkg options to tell to always overwrite the installed destination file, though don’t know why it does NOT work in my test.

via: this thread.

Go to Source
