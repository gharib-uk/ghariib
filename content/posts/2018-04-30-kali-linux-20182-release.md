---
title: "Kali Linux 20182 Release"
date: Mon, 30 Apr 2018 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20182 Release

https://www.kali.org/blog/kali-linux-2018-2-release/images/kali-release.jpg



This Kali release is the first to include the Linux 4.15 kernel, which includes the x86 and x64 fixes for the much-hyped Spectre and Meltdown vulnerabilities. It also includes much better support for AMD GPUs and support for AMD Secure Encrypted Virtualization, which allows for encrypting virtual machine memory such

This Kali release is the first to include the Linux 4.15 kernel, which includes the x86 and x64 fixes for the much-hyped [Spectre and Meltdown](https://meltdownattack.com/) vulnerabilities. It also includes much [better support for AMD GPUs](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=f6705bf959efac87bca76d40050d342f1d212587) and support for [AMD Secure Encrypted Virtualization](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=33e63acc119d15c2fac3e3775f32d1ce7a01021b), which allows for encrypting virtual machine memory such that even the hypervisor can’t access it.

### Easier Metasploit Script Access

If you spend any significant amount of time writing exploits, you are undoubtedly familiar with the various Metasploit scripts that are available, such as **_pattern\_create_**, **_pattern\_offset_**, **_nasm\_shell_**, etc. You are likely also aware that all of these helpful scripts are tucked away under _/usr/share/metasploit-framework/tools/exploit/_, which makes them more than a little difficult to make use of. Fortunately, as of _metasploit-framework\_4.16.34-0kali2_, you can now make use of all these scripts directly as we have included links to all of them in the PATH, each of them prepended with **_msf-_**:

```console
root@kali:~# msf-<tab>
msf-egghunter msf-java_deserializer msf-nasm_shell
msf-exe2vba msf-jsobfu msf-pattern_create
msf-exe2vbs msf-makeiplist msf-pattern_offset
msf-find_badchars msf-md5_lookup msf-pdf2xdp
msf-halflm_second msf-metasm_shell msf-virustotal
msf-hmac_sha1_crack msf-msf_irb_shell
root@kali:~#
root@kali:~# msf-pattern_create -l 50 -s ABC,123
A1A2A3B1B2B3C1C2C3A1A2A3B1B2B3C1C2C3A1A2A3B1B2B3C1
root@kali:~#
```

### Package Updates

In addition to the above changes, there have been updates to a number of applications including [Bloodhound](https://pkg.kali.org/pkg/bloodhound), [Reaver](https://www.kali.org/tools/reaver/), [PixieWPS](https://www.kali.org/tools/pixiewps/), [Burp Suite](https://www.kali.org/tools/burpsuite/), [Hashcat](https://www.kali.org/tools/hashcat/), and more. Since there are far too many packages to include in a default ISO, to see the full list of changes, we encourage you to review the [Kali Changelog](https://bugs.kali.org/changelog_page.php).

### Download Kali Linux 2018.2

If you would like to check out this latest and greatest Kali release, you can find download links for ISOs and Torrents on the [Kali Downloads](https://www.kali.org/get-kali/) page along with links to the OffSec virtual machine and ARM images, which have also been updated to 2018.2. If you already have a Kali installation you’re happy with, you can easily upgrade in place as follows:

```console
root@kali:~# apt update && apt full-upgrade
```

As always, if you encounter any bugs at all, we implore you to open a report on our [bug tracker](https://bugs.kali.org/main_page.php). We can’t fix what we don’t know about.

#### [Source](https://www.kali.org/blog/kali-linux-2018-2-release/)

