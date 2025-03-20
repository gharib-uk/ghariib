---
title: "DBShield – Go Based Database Firewall"
date: 2025-01-02
categories: 
  - "security"
---

DBShield is a Database Firewall written in Go that has protection for MySQL/MariaDB, Oracle and PostgreSQL databases. It works in a proxy fashion inspecting traffic and dropping abnormal queries after a learning period to populate the internal database with regular queries. Learning mode lets any query pass but it records information about it (pattern, username, \[…\]

Go to Source
