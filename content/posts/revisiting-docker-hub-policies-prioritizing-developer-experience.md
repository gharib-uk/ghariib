---
title: "Revisiting Docker Hub Policies: Prioritizing Developer Experience"
date: 2025-03-19
---

At Docker, we are committed to ensuring that Docker Hub remains the best place for developers, engineering teams, and operations teams to build, share, and collaborate. As part of this, we previously announced plans to introduce image pull consumption fees and storage-based billing. After further evaluating how developers use Docker Hub and what will best support the ecosystem, we have refined our approach—one that prioritizes developer experience and enables developers to scale with confidence while reinforcing Docker Hub as the foundation of the cloud-native ecosystem.

## What’s Changing?

We’re making important updates to our previously announced pull limits and storage policies to ensure Docker Hub remains a valuable resource for developers:

- **No More Pull Count Limits or Consumption Charges** – We’re cancelling pull consumption charges entirely. Our focus is on making Docker Hub the best place for developers to build, share, and collaborate—ensuring teams can scale with confidence.
- **Unlimited Pull rates for Paid Users (As Announced Earlier)** – Starting **April 1, 2025**, all **paid Docker subscribers** will have unlimited image pulls (with fair use limits) to ensure a seamless experience.
- **Updated Pull Rate Limits for Free & Unauthenticated Users** – To ensure a reliable and seamless experience for all users, we are updating authenticated and free pull limits:
    - **Unauthenticated users**: Limited to 10 pulls per hour (as announced previously)
    - **Free authenticated users**: Increased to 100 pulls per hour (up from 40 pulls / hour)
    - **System accounts & automation**: As previously shared, automated systems and service accounts can easily authenticate using Personal Access Tokens (PATs) or Organizational Access Tokens (OATs), ensuring access to higher pull limits and a more reliable experience for automated authenticated pulls.
- **Storage Charges Delayed Indefinitely** – Previously, we announced plans to introduce **storage-based billing**, but we have decided to **indefinitely delay any storage charges**. Instead, we are focusing on delivering **new tools** that will allow users to actively manage their storage usage. Once these tools are available, we will assess storage policies in the best interest of our users. **If and when storage charges are introduced, we will provide a six-month notice, ensuring teams have ample time to adjust.**

## Why This Matters

- **The Best Place to Build and Share** – Docker Hub remains **the world’s leading container registry**, trusted by over 20 million developers and organizations. We’re committed to keeping it **the best place to distribute and consume software**.
- **Growing the Ecosystem** – We’re making these changes to **support more developers, teams, and businesses as they scale**, reinforcing Docker Hub as the foundation of the cloud-native world.
- **Investing in the Future** – Our focus is on delivering more capabilities that help developers move faster, **from better storage management to** **strengthening security to better protect the software supply chain**.
- **Committed to Developers** – Every decision we make is about **strengthening the platform and enabling developers to build, share, and innovate without unnecessary barriers**.

We appreciate your feedback, and we’re excited to keep evolving Docker Hub to meet the needs of developers and teams worldwide. Stay tuned for more updates, and as always—happy building! 

​Products, developers, Docker, Docker Hub
