---
title: "Fluentd: How to Use a Parser With Regular Expression (regexp)"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "ai"
  - "cybersecurity"
  - "security"
  - "soc"
---

![](https://socprime.com/wp-content/uploads/Fluentd_regexp-400x234.jpg)

This guide explains configuring Fluentd to extract structured data from unstructured log messages using the parser plugin with a regular expression (regexp). If you need to extract specific fields, such as log\_source and index, from a log message, you can do this as follows.

Input Log:

```

{
  "message": "Log source 'WinCollect DSM - SRV-AD-001' has stopped emitting events"
}
```

Configuration:

```
<filter **>
  @type parser
  key_name message
  reserve_data true
  <parse>
    @type regexp
    expression /'(?<log_source>[^']+)s-s(?<index>[^']+)'/
  </parse>
</filter>
```

**Explanation**:

- `key_name message`: Specifies that the `message` field should be parsed.
- `reserve_data true`: Keeps the original `message` field along with the extracted fields.
- `regexp expression`:
    - `(?<log_source>[^']+)`: Captures the text before `-` as `log_source`.
    - `(?<index>[^']+)`: Captures the text after `-` as `index`.

Output Log:

```
{
  "message": "Log source 'WinCollect DSM - SRV-AD-001' has stopped emitting events",
  "log_source": "WinCollect DSM",
  "index": "SRV-AD-001"
}
```

If you need to extract fields such as `timestamp`, `level`, `module`, and message from logs with timestamps, you can do this as follows:

Input Log:

```
{
  "message": "2024-12-18 10:15:30 ERROR [Auth] Login failed for user 'jdoe'"
}
```

Configuration:

```
<filter **>
  @type parser
  key_name message
  reserve_data true
  <parse>
    @type regexp
    expression /(?<timestamp>d{4}-d{2}-d{2} d{2}:d{2}:d{2})s+(?<level>[A-Z]+)s+[(?<module>[^]]+)]s+(?<message>.*)/
  </parse>
</filter>
```

**Explanation**:

- `(?<timestamp>d{4}-d{2}-d{2} d{2}:d{2}:d{2})`: Extracts the timestamp.
- `(?<level>[A-Z]+)`: Captures the log level (e.g., `ERROR`).
- `(?<module>[^]]+)`: Extracts the module name (e.g., `Auth`).
- `(?<message>.*)`: Captures the remaining log message.

Output Log:

```
{
  "message": "2024-12-18 10:15:30 ERROR [Auth] Login failed for user 'jdoe'",
  "timestamp": "2024-12-18 10:15:30",
  "level": "ERROR",
  "module": "Auth",
  "message": "Login failed for user 'jdoe'"
}
```

If you need to extract key-value pairs from a log message, you can do this as follows:

Input Log:

```
{
  "message": "user=jdoe status=failed ip=192.168.12.1"
}
```

Configuration:

```
<filter **>
  @type parser
  key_name message
  reserve_data true
  <parse>
    @type regexp
    expression /user=(?<user>w+)s+status=(?<status>w+)s+ip=(?<ip>[^s]+)/
  </parse>
</filter>
```

**Explanation**:

- `(?<user>w+)`: Captures the username.
- `(?<status>w+)`: Extracts the status (e.g., `failed`).
- `(?<ip>[^s]+)`: Captures the IP address.

Output Log:

```
{
  "message": "user=jdoe status=failed ip=192.168.12.1",
  "user": "jdoe",
  "status": "failed",
  "ip": "192.168.12.1"
}
```

  
  

The post Fluentd: How to Use a Parser With Regular Expression (regexp) appeared first on SOC Prime.

Go to Source
