---
title: "USN-7358-1 PostgreSQL vulnerabilities"
date: Wed, 19 Mar 2025 12:15:54 +0000
draft: false
type: posts
categories: 
- 
---
# USN-7358-1 PostgreSQL vulnerabilities

<br/>

<br/>
Wolfgang Walther discovered that PostgreSQL incorrectly tracked tables with row security. A remote attacker could possibly use this issue to perform forbidden reads and modifications. (CVE-2024-10976) Jacob Champion discovered that PostgreSQL clients used untrusted server error messages. An attacker that is able to intercept network communications could possibly use this issue to inject error messages that could be interpreted as valid query results. (CVE-2024-10977) Tom Lane discovered that PostgreSQL incorrectly handled certain privilege assignments. A remote attacker could possibly use this issue to view or change different rows from those intended. (CVE-2024-10978) Coby Abrams discovered that PostgreSQL incorrectly handled environment variables. A remote attacker could possibly use this issue to execute arbitrary code. (CVE-2024-10979)

#### [Source](https://ubuntu.com/security/notices/USN-7358-1)

<br/>
---
