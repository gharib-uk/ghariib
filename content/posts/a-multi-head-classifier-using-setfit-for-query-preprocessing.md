---
title: "A multi-head classifier using SetFit for query preprocessing"
date: 2025-02-03
---

Query preprocessing is a crucial step in customer-centric applications, ensuring accurate routing and action decisions. Instead of training separate models, I useSetFit with a multi-head classifier— a single shared embedding space with independent classification heads.

Each head specialises in one task, allowing task-specific learning while maintaining efficiency through shared representations. Positive and negative pairs are created using (intent, domain, hitl(human in the loop)) tuples for contrastive learning, ensuring the model effectively distinguishes related queries. This structured approach balances efficiency and flexibility, making it ideal for real-time applications where query classification must be both accurate and scalable. This is just an example. You may modify it as per your needs.

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbobpmcm719k8v6grll9u.png)

Link to the Repo

Go to Source
