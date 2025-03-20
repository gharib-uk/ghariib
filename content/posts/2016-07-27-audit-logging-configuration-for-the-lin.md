---
title: "Audit Logging Configuration for the Linux Environment"
date: 2016-07-26T21:52:00.000-04:00
draft: false
type: posts
---
# Audit Logging Configuration for the Linux Environment

<br/>

<br/>
One challenge to performing a proper incident investigation is dealing with missing event logs. Part of a healthy SOC posture is ensuring that you have the proper audit logging settings to ensure that you log what is needed tomorrow.  
  
Windows has a very [well](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations) [defined](http://static1.squarespace.com/static/552092d5e4b0661088167e5c/t/5681a0fa0e4c119b0ee82345/1451335930842/Windows+Logging+Cheat+Sheet_ver_Jan_2016.pdf) [audit](http://www.solarwinds.com/documentation/lem/docs/html/content/KB%20Articles%20-%20updated/lem-windows-audit-best%20practice.htm) [policy](https://www.ultimatewindowssecurity.com/wiki/WindowsSecuritySettings/Recommended-Baseline-Audit-Policy-for-Windows-Server-2008), but when I was trying to find an audit policy for the Linux audit system I found it much more difficult than expected. One excellent resource I found is the Defense Information Systems Agency (DISA)'s Security Technical Implementation Guide (STIG) for Red Hat Enterprise Linux 6, which can be found [here](https://web.nvd.nist.gov/view/ncp/repository/checklist/download?id=12321&checklistId=438), and the STIG Viewer can be found [here](http://iase.disa.mil/stigs/Pages/stig-viewing-guidance.aspx).  
  
To open the STIG, open the JAR file by double clicking on it (or running _java -jar <filename>_ from the CLI.) Next, go to _file_ --> _Import STIG_ and traverse to the STIG file location. Next, change the file extension box from XML to ZIP:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEihaYH78jIsSUdroEmJRRc1-G0vTQ0x3Duv4VHfI9P_iPsPUsWC6JVzRTiW_GMT3vkOxQgLgnrQKy4_mL1l3LeZzUwFUsUjNOSN1sOTTmWJPImYzRw2d1DRHFhq5Mo_OAWRV4weMO-nfWZD/s320/File+Extension+-+Zip.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEihaYH78jIsSUdroEmJRRc1-G0vTQ0x3Duv4VHfI9P_iPsPUsWC6JVzRTiW_GMT3vkOxQgLgnrQKy4_mL1l3LeZzUwFUsUjNOSN1sOTTmWJPImYzRw2d1DRHFhq5Mo_OAWRV4weMO-nfWZD/s1600/File+Extension+-+Zip.PNG)

The RHEL STIG will open with 236 security-based rules that outline how the Department of Defense audits their RHEL systems to ensure that they adhere to their security standards, and I would highly recommend going through it if you manage a RHEL system. But today we will be focusing on the RHEL audit configuration recommendations. Enter the word audit in the Filter window pane of the STIG viewer (I typically don't push enter as I find that this works better.) From there you should filter for rules that contain the word, well, audit  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEibQMzonTbe4IhujcEqQe54F1oa0h4P8D9eAyoeBdUjD4vYQOvXdUm0jENIIyiQc53jbvXE9UVz5LJaPYlpX8-RZqOrW6LJ72Nd5pSs4U_Fo03NpIDi72CPlTsKaqN6JLr-EMeqGo-2Adyk/s320/RHEL+DISA+STIG.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEibQMzonTbe4IhujcEqQe54F1oa0h4P8D9eAyoeBdUjD4vYQOvXdUm0jENIIyiQc53jbvXE9UVz5LJaPYlpX8-RZqOrW6LJ72Nd5pSs4U_Fo03NpIDi72CPlTsKaqN6JLr-EMeqGo-2Adyk/s1600/RHEL+DISA+STIG.png)

The Linux system uses the auditd service to create and log audit events within the system. The first step is to ensure that the auditd service is enabled:  

```
# systemctl is-active auditd.service 
Active: active (running) since Tue 2015-01-27 19:41:23 EST; 22h ago
```

Next, the auditd service uses the audit.rules configuration file to determine what actions to audit. Details on the CentOS audit configuration file can be found on Digital Ocean's tutorials [here](https://www.digitalocean.com/community/tutorials/how-to-use-the-linux-auditing-system-on-centos-7) and [here](https://www.digitalocean.com/community/tutorials/how-to-write-custom-system-audit-rules-on-centos-7). As for what to audit, the STIG recommends that we run a search on the file system to find all programs with execution function capabilities and add them to the audit configuration file. To find relevant setuid and setgid programs, use the following command once for each local partition:  

```
find / -xdev \( -perm -4000 -o -perm -2000 \) -type f | awk '{print "-a always,exit -F path=" $1 " -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged" }'
```

`This outputs a series of audit configuration parameters that can be placed in the /etc/audit/rules.d/audit.rules file to ensure that using said commands/binaries is a logged event. An example output for my Linux box shows:`  

\-a always,exit -F path=/usr/bin/wall -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/chage -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/newgrp -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/chfn -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/write -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/chsh -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/mount -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/at -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/su -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/umount -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/passwd -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/sudo -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/gpasswd -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/pkexec -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/crontab -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/locate -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/ssh-agent -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/bin/staprun -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/sbin/pam\_timestamp\_check -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/sbin/unix\_chkpwd -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/sbin/netreport -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/sbin/usernetctl -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/sbin/userhelper -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/sbin/postdrop -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/sbin/postqueue -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/lib/polkit-1/polkit-agent-helper-1 -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/lib64/dbus-1/dbus-daemon-launch-helper -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/libexec/utempter/utempter -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/libexec/openssh/ssh-keysign -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged
-a always,exit -F path=/usr/libexec/abrt-action-install-debuginfo-to-abrt-cache -F perm=x -F auid>=1000 -F auid!=4294967295 -k privileged

Going through the rest of the STIG shows eight categories of audit logging settings that I have pasted into this very pretty and convenient table:

```

 
 
 
 
 

  Log Group
  Activity
  Audit Trigger
  Details
 

  
1

  System time alteration
  settimeofday
  -a always,exit
  -F arch=b32 -S settimeofday -k audit_time_rules

    -a always,exit -F arch=b64 -S settimeofday -k audit_time_rules
 

  stime
  -a
  always,exit -F arch=b32 -S stime -k audit_time_rules 

    -a always,exit -F arch=b64 -S adjtimex -S settimeofday -S clock_settime -k
  audit_time_rules
 

  clock_settime
  -a
  always,exit -F arch=b32 -S clock_settime -k audit_time_rules

    -a always,exit -F arch=b64 -S clock_settime -k audit_time_rules
 

  localtime
  -w /etc/localtime -p
  wa -k audit_time_rules
 

  
2

  Account Changes
  group
  -w /etc/group -p wa
  -k audit_account_changes
 

  passwd
  -w /etc/passwd -p wa
  -k audit_account_changes
 

  gshadow
  -w /etc/gshadow -p wa
  -k audit_account_changes
 

  shadow
  -w /etc/shadow -p wa
  -k audit_account_changes
 

  opasswd
  -w
  /etc/security/opasswd -p wa -k audit_account_changes
 

  
3

  Network Modifications
  sethostname
  -a
  always,exit -F arch=B32 -S sethostname -S setdomainname -k
  audit_network_modifications

    -a always,exit -F arch=B64 -S sethostname -S setdomainname -k
  audit_network_modifications
 

  issue
  -w /etc/issue -p wa
  -k audit_network_modifications
 

  issue.net
  -w /etc/issue.net -p
  wa -k audit_network_modifications
 

  hosts
  -w /etc/hosts -p wa
  -k audit_network_modifications
 

  network
  -w
  /etc/sysconfig/network -p wa -k audit_network_modifications
 

  
4

  Mandatory Access
  Control (MAC) configuration (SELinux)
  MAC-policy
  -w /etc/selinux/ -p
  wa -k MAC-policy
 

  
5

  High Priority Commands
  chmod
  -a
  always,exit -F arch=b32 -S chmod -F auid>=500 -F auid!=4294967295 -k
  perm_mod 

    -a always,exit -F arch=b32 -S chmod -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S chmod -F auid>=500 -F auid!=4294967295 -k
  perm_mod 

    -a always,exit -F arch=b64 -S chmod -F auid=0 -k perm_mod
 

  chown
  -a
  always,exit -F arch=b32 -S chown -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b32 -S chown -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S chown -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b64 -S chown -F auid=0 -k perm_mod
 

  fchmod
  -a
  always,exit -F arch=b32 -S fchmod -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b32 -S fchmod -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S fchmod -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b64 -S fchmod -F auid=0 -k perm_mod
 

  fchmodat
  -a
  always,exit -F arch=b32 -S fchmodat -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b32 -S fchmodat -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S fchmodat -F auid>=500 -F auid!=4294967295
  -k perm_mod

    -a always,exit -F arch=b64 -S fchmodat -F auid=0 -k perm_mod
 

  fchown
  -a
  always,exit -F arch=b32 -S fchown -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b32 -S fchown -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S fchown -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b64 -S fchown -F auid=0 -k perm_mod
 

  fchownat
  -a
  always,exit -F arch=b32 -S fchownat -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b32 -S fchownat -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S fchownat -F auid>=500 -F auid!=4294967295
  -k perm_mod

    -a always,exit -F arch=b64 -S fchownat -F auid=0 -k perm_mod
 

  fremovexattr
  -a
  always,exit -F arch=b32 -S fremovexattr -F auid>=500 -F auid!=4294967295
  -k perm_mod

    -a always,exit -F arch=b32 -S fremovexattr -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S fremovexattr -F auid>=500 -F
  auid!=4294967295 -k perm_mod

    -a always,exit -F arch=b64 -S fremovexattr -F auid=0 -k perm_mod
 

  fsetxattr
  -a
  always,exit -F arch=b32 -S fsetxattr -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b32 -S fsetxattr -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S fsetxattr -F auid>=500 -F auid!=4294967295
  -k perm_mod

    -a always,exit -F arch=b64 -S fsetxattr -F auid=0 -k perm_mod
 

  lchown
  -a
  always,exit -F arch=b32 -S lchown -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b32 -S lchown -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S lchown -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b64 -S lchown -F auid=0 -k perm_mod
 

  lremovexattr
  -a
  always,exit -F arch=b32 -S lremovexattr -F auid>=500 -F auid!=4294967295
  -k perm_mod

    -a always,exit -F arch=b32 -S lremovexattr -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S lremovexattr -F auid>=500 -F
  auid!=4294967295 -k perm_mod

    -a always,exit -F arch=b64 -S lremovexattr -F auid=0 -k perm_mod
 

  lsetxattr
  -a
  always,exit -F arch=b32 -S lsetxattr -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b32 -S lsetxattr -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S lsetxattr -F auid>=500 -F auid!=4294967295
  -k perm_mod

    -a always,exit -F arch=b64 -S lsetxattr -F auid=0 -k perm_mod
 

  removexattr
  -a
  always,exit -F arch=b32 -S removexattr -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b32 -S removexattr -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S removexattr -F auid>=500 -F
  auid!=4294967295 -k perm_mod

    -a always,exit -F arch=b64 -S removexattr -F auid=0 -k perm_mod
 

  setxattr
  -a
  always,exit -F arch=b32 -S setxattr -F auid>=500 -F auid!=4294967295 -k
  perm_mod

    -a always,exit -F arch=b32 -S setxattr -F auid=0 -k perm_mod

    -a always,exit -F arch=b64 -S setxattr -F auid>=500 -F auid!=4294967295
  -k perm_mod

    -a always,exit -F arch=b64 -S setxattr -F auid=0 -k perm_mod
 

  mount
  -a
  always,exit -F arch=b32 -S mount -F auid>=500 -F auid!=4294967295 -k
  export

    -a always,exit -F arch=b32-S mount -F auid=0 -k export

    -a always,exit -F arch=b64 -S mount -F auid>=500 -F auid!=4294967295 -k
  export

    -a always,exit -F arch=b64 -S mount -F auid=0 -k export
 

  unlink
  -a
  always,exit -F arch=b32 -S rmdir -S unlink -S unlinkat -S rename -S renameat
  -F auid>=500 -F auid!=4294967295 -k delete

    -a always,exit -F arch=b32 -S rmdir -S unlink -S unlinkat -S rename -S
  renameat -F auid=0 -k delete

    -a always,exit -F arch=b64 -S rmdir -S unlink -S unlinkat -S rename -S
  renameat -F auid>=500 -F auid!=4294967295 -k delete

    -a always,exit -F arch=b64 -S rmdir -S unlink -S unlinkat -S rename -S
  renameat -F auid=0 -k delete
 

  lnsmod
  -w
  /sbin/insmod -p x -k modules

    -w /sbin/rmmod -p x -k modules

    -w /sbin/modprobe -p x -k modules

    -a always,exit -F arch=b32 -S init_module -S delete_module -k modules

    -a always,exit -F arch=b64 -S init_module -S delete_module -k modules
 

  
6

  Unauthorized File
  Access Attempt
  open
  -a
  always,exit -F arch=b32 -S creat -S open -S openat -S truncate -S ftruncate
  -F exit=-EACCES -F auid>=500 -F auid!=4294967295 -k access

    -a always,exit -F arch=b32 -S creat -S open -S openat -S truncate -S
  ftruncate -F exit=-EPERM -F auid>=500 -F auid!=4294967295 -k access

    -a always,exit -F arch=b32 -S creat -S open -S openat -S truncate -S
  ftruncate -F exit=-EACCES -F auid=0 -k access

    -a always,exit -F arch=b32 -S creat -S open -S openat -S truncate -S
  ftruncate -F exit=-EPERM -F auid=0 -k access

    -a always,exit -F arch=b64 -S creat -S open -S openat -S truncate -S
  ftruncate -F exit=-EACCES -F auid>=500 -F auid!=4294967295 -k access

    -a always,exit -F arch=b64 -S creat -S open -S openat -S truncate -S
  ftruncate -F exit=-EPERM -F auid>=500 -F auid!=4294967295 -k access

    -a always,exit -F arch=b64 -S creat -S open -S openat -S truncate -S
  ftruncate -F exit=-EACCES -F auid=0 -k access

    -a always,exit -F arch=b64 -S creat -S open -S openat -S truncate -S
  ftruncate -F exit=-EPERM -F auid=0 -k access
 

  
7

  Sudoers
  Sudoers
  -w /etc/sudoers -p wa
  -k actions
 

  
8

  All privileged
  Commands
  Priv commands
  To
  audit use of all privileged commands run:

          # find PART -xdev \( -perm
  -4000 -o -perm -2000 \) -type f | awk '{print \ "-a always,exit -F
  path=" $1 " -F perm=x -F auid>=1000 -F auid!=4294967295 \ -k
  privileged" }'

    

    Next, add those lines to the /etc/audit/audit.rules file.

```

Now when commands are run /var/log/audit/audit.log. Running the command chmod produces the following output:  

\[epicism@rhel audit\]# chmod 777 test
\[epicism@rhel audit\]# tail-f /var/log/audit/audit.log
type=SYSCALL msg=audit(1469583828.606:21849): arch=c000003e syscall=268 success=yes exit=0 a0=ffffffffffffff9c a1=bd00f0 a2=1ff a3=7fff145eaaf0 items=1 ppid=6170 pid=13606 auid=0 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=pts0 ses=1 comm="chmod" exe="/usr/bin/chmod" subj=unconfined\_u:unconfined\_r:unconfined\_t:s0-s0:c0.c1023 key="perm\_mod"
type=CWD msg=audit(1469583828.606:21849):  cwd="/var/log/audit"
type=PATH msg=audit(1469583828.606:21849): item=0 name="test" inode=67225482 dev=fd:00 mode=0100777 ouid=0 ogid=0 rdev=00:00 obj=unconfined\_u:object\_r:auditd\_log\_t:s0 objtype=NORMAL

No common SIEM can parse this, but Splunk could probably parse it out pretty easily. In the least I would recommend backing up the audit.log files to a repository for audit purposes. In any case, I hope that the post helps to set a baseline audit configuration file on RHEL.  
  
Update: Our friend Dixie Flatline was kind enough to share the [Audisp plugin](https://github.com/gdestuynder/audisp-cef) which converts Linux audit events to CEF (ArcSight's Comment Event Format standard) which can be interpreted by ArcSight directly without a parser.

#### [Source](https://www.redblue.team/feeds/6277394474793436674/comments/default)

<br/>
---
