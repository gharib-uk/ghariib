---
title: "After More than 5 Years of Maintaining WordPress Websites, Here’s What I’ve Learned"
date: 2025-01-23
---

# Introduction

If someone tells you, "Don’t worry, my website is secure," then you’re not paranoid enough—this guide is for you. As scammers and hackers become increasingly sophisticated, securing your WordPress website is more critical than ever. Below, I’ve compiled a practical guide to help you mitigate common WordPress vulnerabilities and exploits.

## 1\. Use Secure and Maintained WordPress Themes

### Avoid Stolen Plugins or Themes

Never use pirated or unsupported themes or plugins—they’re often compromised. Avoid one-off sales for plugins if they don’t offer ongoing support.

### Choose Well-Maintained Themes

Opt for themes that are regularly updated and supported. For corporate projects, consider using themes provided or adapted through WPVIP for enhanced security and support.

### Use WordPress as a Headless CMS

Modern tools like NextJS and the WP Rest API allow you to use WordPress as a backend while leveraging the power of front-end frameworks like React or Angular. This approach can improve performance and security.

## 2\. Implement Robust Security Measures

### Regular Maintenance

Always keep WordPress core, themes, and plugins up to date. Security patches and updates are essential to protect against known vulnerabilities.

### Web Penetration Testing

- **WPScan**: Use this tool to scan for WordPress-specific vulnerabilities.
- **BurpSuite**: Perform security inspections on headers and cookies.
- **Nmap**: Run Nmap on your server’s IP to check for unnecessary open ports (e.g., SSH or WP-CLI). Restrict or disable these ports if not needed.

### SSH & WP-CLI Hardening

Lock down SSH access and handle WP-CLI carefully to prevent potential exploits.

### Disable Unused API Routes

Turn off any API endpoints you aren’t using to limit exposure. For example, disable `rest_route=/wp/v2/users` to prevent username leaks.

### Scan for Leaked Information

Regularly check posts, comments, and other content for accidentally exposed usernames or sensitive data.

## 3\. Rate Limit Users

Your users don’t need unlimited requests. Implement rate limiting to prevent abuse. For example, limit requests to 500 per minute and block excessive traffic. This can help mitigate DDoS attacks.

### Example of a DDoS Attack Script

```
function floodImagesXYZ() {
  var TARGET = ""; // ADD TARGET URI
  var URI = "/index.php?";
  var pic = new Image();
  var rand = Math.floor(Math.random() * 10000000000000000000000);
  try {
    pic.src = "http://" + TARGET + URI + rand + "=val";
  } catch {
    (error) => {
      console.log(error);
      console.log("Error in:", URI);
    };
  }
}
setInterval(floodImagesXYZ, 10);
```

This script demonstrates how easily a server can be overwhelmed by repeated requests. Implementing rate limiting and using tools like **Cloudflare** can help absorb such attacks.

## 4\. Use Hardening Tools

### Block Unnecessary File Uploads

If you don’t need to upload files via PHP, block that functionality to reduce vulnerabilities.

### Additional Hardening

Implement server configurations and tools to mitigate common threats, such as file permission issues or vulnerable script executions.

## 5\. Manage Builder Editors Carefully

If you’re using page builders like **Divi**, **Elementor**, or **WPBakery**, disable hardening tools that block PHP uploads while making changes. For example, Elementor might throw errors like `the_content()` not being present if the firewall blocks legitimate requests.

## 6\. Custom Code Best Practices

### Review Custom Code

Always review custom scripts, widgets, and third-party integrations to ensure they’re secure.

### Use Development Tools

- **Plugin Check Plugin**: For reviewing plugins before submission to the WordPress directory.
- **Envato Theme Checker**: For ensuring theme security and compliance.
- **PHPUnit**: For unit testing custom code.
- **PHP Code Beautifier**: For adhering to PHP coding standards.

### Escape Data and Validate Nonces

Always escape output and validate nonces when sending information to prevent security breaches.

## 7\. Install a Web Application Firewall (WAF)

### Use a WAF like Wordfence

A good WAF filters out malicious traffic, prevents brute-force attacks, and safeguards against common web application vulnerabilities.

### Proactive Defense

Enable active monitoring on your WAF to log and block suspicious activity.

## 8\. Leverage Cloudflare for Extra Protection

### Connect Your Site Through Cloudflare

Cloudflare adds a layer of protection by filtering out malicious traffic before it reaches your server.

### DDoS Mitigation

Cloudflare is especially useful for blocking DDoS attacks and protecting your site from traffic surges.

## 9\. Backup Regularly

### Keep Up-to-Date Backups

Ensure you always have current backups of your files and database.

### Know Your Restoration Process

Familiarize yourself with the restoration process to quickly recover your site if needed.

## 10\. Enable Two-Factor Authentication (2FA)

### Use 2FA for Admin Access

Add an extra layer of protection by enabling Two-Factor Authentication (2FA) for all admin accounts. This prevents unauthorized access even if login credentials are compromised.

## 11\. Implement reCAPTCHA for Forms and Login

### reCAPTCHA v3 for Login

Protect your login page against bot logins and brute-force attacks without disrupting the user experience.

### reCAPTCHA v2 for Forms

Use reCAPTCHA v2 for contact forms, registration forms, and other user inputs to prevent spam submissions.

## Conclusion

Even with all these security measures in place, no system is 100% secure. The key is to make your website as secure as possible and remain vigilant. Regularly back up your site, monitor for vulnerabilities, and stay informed about emerging threats.

Remember, hackers don’t target businesses personally—it’s purely about money. Large businesses are often attacked the most due to their visibility and potential financial rewards. That’s why many companies offer bug bounty programs to mitigate risks further.

Stay proactive, and don’t underestimate the importance of ongoing maintenance and security.

## References and Further Reading

1. WordPress Security Team
2. WPScan Vulnerability Database
3. Cloudflare DDoS Protection
4. What is a Layer 7 DDoS Attack? - Cloudflare
5. Application Layer DDoS Attack - Cloudflare
6. Wordfence Web Application Firewall
7. Malcure Security Tools
8. NextJS Documentation
9. WP Rest API Handbook
10. Nmap Network Scanning
11. BurpSuite Security Testing
12. Google reCAPTCHA
13. PHPUnit Testing Framework

Go to Source
