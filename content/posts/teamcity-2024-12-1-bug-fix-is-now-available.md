---
title: "TeamCity 2024.12.1 Bug Fix Is Now Available"
date: 2025-01-22
categories: 
  - "development"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "linux"
  - "software"
---

We’re excited to announce the release of TeamCity On-Premises 2024.12.1, a bug fix update that resolves over 80 issues reported by users. This version includes crucial fixes across multiple areas, ensuring enhanced performance, stability, and security for your CI/CD pipelines. Some highlights of this release include: We recommend upgrading to apply the latest improvements and \[…\]

We’re excited to announce the release of **TeamCity On-Premises 2024.12.1**, a bug fix update that resolves over 80 issues reported by users. This version includes crucial fixes across multiple areas, ensuring enhanced performance, stability, and security for your CI/CD pipelines.

Some highlights of this release include:

- **Resolved truncated build tags**, addressing an issue that impacted tag visibility.

- **Enhanced VCS checkout**, ensuring revision computation considers all VCS root variations.

- **Fixed possible agent hang-ups** during artifact publishing.

- **Support for MySQL 8.4** by adding `allowPublicKeyRetrieval` to the database connection URL (TW-91529).

- **Fixed SSH agent build feature issues** on Windows (TW-85769).

We recommend upgrading to apply the latest improvements and security fixes to your TeamCity server.

### **Why update?**

Staying up to date with minor releases ensures your TeamCity instance benefits from:

- Performance improvements.

- Better compatibility with integrations.

- Faster, more stable builds.

- Enhanced security for your workflows.

### **Compatibility**

TeamCity 2024.12.1 shares the same data format as all 2024.12.x releases. You can upgrade or downgrade within this series without the need for backup and restoration.

### **How to upgrade**

1. Use the **automatic update** feature in your current TeamCity version.

4. Download the latest version directly from the JetBrains website.

7. Pull the updated TeamCity Docker image.

### **Need help?**

Thank you for reporting issues and providing feedback! If you have questions or run into any problems, please let us know via the TeamCity Forum or Issue Tracker.

Happy building!

Go to Source
