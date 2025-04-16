---
title: "Kali Essential Security"
date: 2025-04-11
draft: false
type: posts
categories:
  - kali_linux, kali
tags:
  - kali, kali-linux
summary: "Hardening Kali Linux"
---


Hardening Kali Linux is essential for maintaining security, especially since it is a penetration testing distro that can be a target for attackers.


---

## **0. Change kali-rolling to kali-last-snapshot
It is not explicitly associated with security but it affects it implicitly.

In addition to this it affects the stability of whole system.

```bash
deb https://kali.download kali-last-snapshot <keep others here>
```
---

## **1. Update and Upgrade Regularly**
Ensure your system is always updated with the latest security patches.
```bash
sudo apt update && sudo apt full-upgrade -y
```
For kernel updates:
```bash
sudo apt dist-upgrade -y
```
Remove unnecessary packages:
```bash
sudo apt autoremove -y && sudo apt clean
```

---

## **2. Secure User Accounts and Authentication**
### **Disable Root Login**
Kali uses `kali` as the default user. Ensure root login is disabled.
```bash
sudo passwd -l root
```

### **Use Strong Passwords**
Use a strong password or configure password complexity policies:
```bash
sudo apt install libpam-pwquality
sudo nano /etc/security/pwquality.conf
```
Modify:
```
minlen = 12
dcredit = -1
ucredit = -1
lcredit = -1
ocredit = -1
```

### **Enable Two-Factor Authentication (2FA)**
```bash
sudo apt install libpam-google-authenticator
google-authenticator
```
Configure `/etc/pam.d/sshd`:
```
auth required pam_google_authenticator.so
```
Restart SSH:
```bash
sudo systemctl restart ssh
```

---

## **3. Configure SSH Securely**
Edit SSH config:
```bash
sudo nano /etc/ssh/sshd_config
```
Modify:
```
PermitRootLogin no
PasswordAuthentication no
PermitEmptyPasswords no
UsePAM yes
MaxAuthTries 3
AllowUsers your_username
```
Restart SSH:
```bash
sudo systemctl restart ssh
```

---

## **4. Enable Firewall (UFW)**
```bash
sudo apt install ufw -y
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp  # If using SSH
sudo ufw enable
sudo ufw status verbose
```

---

## **5. Enable AppArmor or SELinux**
AppArmor (default in Kali):
```bash
sudo apt install apparmor apparmor-profiles apparmor-utils -y
sudo systemctl enable --now apparmor
```

For SELinux (optional):
```bash
sudo apt install selinux-basics selinux-policy-default auditd -y
sudo selinux-activate
sudo reboot
```

---

## **6. Configure Automatic Security Updates**
Edit:
```bash
sudo nano /etc/apt/apt.conf.d/20auto-upgrades
```
Add:
```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

---

## **7. Remove Unnecessary Services**
List enabled services:
```bash
systemctl list-unit-files --type=service | grep enabled
```
Disable unneeded ones:
```bash
sudo systemctl disable avahi-daemon
sudo systemctl disable bluetooth
sudo systemctl disable cups
```

---

## **8. Harden Networking**
### **Disable IPv6 (if not needed)**
Edit GRUB:
```bash
sudo nano /etc/default/grub
```
Modify:
```
GRUB_CMDLINE_LINUX="ipv6.disable=1"
```
Update GRUB:
```bash
sudo update-grub && sudo reboot
```

### **Enable SYN Flood Protection**
```bash
echo "net.ipv4.tcp_syncookies = 1" | sudo tee -a /etc/sysctl.conf
```

### **Disable ICMP Responses (Optional)**
```bash
echo "net.ipv4.icmp_echo_ignore_all = 1" | sudo tee -a /etc/sysctl.conf
```

Apply changes:
```bash
sudo sysctl -p
```

---

## **9. Secure Bootloader**
Prevent unauthorized access by setting a GRUB password:
```bash
sudo grub-mkpasswd-pbkdf2
```
Copy the generated hash and add it to `/etc/grub.d/40_custom`:
```bash
sudo nano /etc/grub.d/40_custom
```
Add:
```
set superusers="root"
password_pbkdf2 root <hashed-password>
```
Update GRUB:
```bash
sudo update-grub
```

---

## **10. Use Encrypted Disk or LUKS for Sensitive Data**
Encrypt a partition:
```bash
sudo cryptsetup luksFormat /dev/sdX
sudo cryptsetup luksOpen /dev/sdX secure_data
```
For full disk encryption, use LUKS during installation.

---

## **11. Install an Intrusion Detection System (IDS)**
### **AIDE (File Integrity Monitoring)**
```bash
sudo apt install aide -y
sudo aideinit
sudo mv /var/lib/aide/aide.db.new /var/lib/aide/aide.db
```
Check system integrity:
```bash
sudo aide --check
```

### **Tripwire (Alternative IDS)**
```bash
sudo apt install tripwire -y
```
Initialize and configure rules.

---

## **12. Harden Browser and Online Privacy**
- Use **Firefox with NoScript and uBlock Origin**.
- Enable **DNS over HTTPS (DoH)** in Firefox.
- Configure **Tor and VPN** for anonymous browsing.

---

## **13. Secure Logging and Monitoring**
### **Enable Log Rotation**
```bash
sudo nano /etc/logrotate.conf
```
Ensure logs are rotated and archived.

### **Use AuditD for Logging**
```bash
sudo apt install auditd -y
sudo systemctl enable --now auditd
```
Check logs:
```bash
sudo ausearch -m avc
```

---

## **14. Restrict USB Access (Optional)**
To disable USB storage:
```bash
echo "blacklist usb-storage" | sudo tee /etc/modprobe.d/usb-storage.conf
```
Apply changes:
```bash
sudo update-initramfs -u && sudo reboot
```

---

## **15. Physical Security Measures**
- **Disable unattended access** (lock screen with `Ctrl + Alt + L`).
- **Use BIOS/UEFI password**.
- **Disable booting from USB/CD** in BIOS.

---

## **16. Sandboxing and Isolation**
### **Firejail for Application Isolation**
```bash
sudo apt install firejail -y
firejail --seccomp firefox
```

---

## **17. Encrypt Swap and TMP**
Edit `/etc/fstab`:
```bash
sudo nano /etc/fstab
```
Add:
```
tmpfs /tmp tmpfs defaults,noexec,nosuid 0 0
```
For encrypted swap:
```bash
sudo apt install cryptsetup -y
sudo cryptsetup luksFormat /dev/sdX
sudo cryptsetup luksOpen /dev/sdX swap
mkswap /dev/mapper/swap
swapon /dev/mapper/swap
```

---

## **18. Remove Unnecessary Tools**
Since Kali comes with many tools, remove what you don’t use:
```bash
sudo apt remove wireshark metasploit-framework -y
```

---

## **19. Enable MAC Address Randomization**
For better anonymity:
```bash
sudo nano /etc/NetworkManager/conf.d/wifi_scan-rand-mac.conf
```
Add:
```
[device]
wifi.scan-rand-mac-address=yes
```
Restart NetworkManager:
```bash
sudo systemctl restart NetworkManager
```

---

## **20. Use a Hardened Kernel (Optional)**
Consider using the **grsecurity** or **linux-hardened** kernel.

---
