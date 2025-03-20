---
title: "Firebase CLI Installer Making Calls to Google Analytics"
date: 2025-01-06
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
tags: 
  - "analytics"
  - "firebase"
  - "google"
---

Firebase is a mobile and web application development platform provided by Google. One of the tools available for the platform is the Firebase CLI tool (GitHub repo) which helps developers interact with the platform from command line. An automatic install script is offered among other options, which allows installation of the CLI tool via the “curl | bash” pattern as follows:

```
curl -sL https://firebase.tools | bash
```

As part of our ongoing research into supply chain attacks, we have been looking into bash installer scripts that make calls to external systems. First, there is no way to verify that the installer is legit, However, to our surprise, we also found that this script makes calls to Google Analytics as part of the installation process. There is no sensitive data being collected but Google may still be collecting IP addresses of users installing the CLI. The source code for the installer script can be found here:

https://firebase.tools/

While this can be disabled, the documentation to do so is hard to find and is embedded within the installer script itself. We hope that Google will make this documentation more clear in the future. In any case, here is the documentation:

![](https://wwws.nightwatchcybersecurity.com/wp-content/uploads/2021/05/screen-shot-2021-05-25-at-8.08.40-am.png?w=630)

The actual code that makes these calls can be found here:

![](https://wwws.nightwatchcybersecurity.com/wp-content/uploads/2021/05/screen-shot-2021-05-25-at-8.10.15-am.png?w=625)

And here are all the analytics events triggered within the script:

```
send_analytics_event start
...
send_analytics_event uninstall_npm
...
send_analytics_event uninstall
...
send_analytics_event already_installed
...
send_analytics_event upgrade
...
send_analytics_event "missing_platform_$UNAME"
...
send_analytics_event failure
...
send_analytics_event missing_path
...
send_analytics_event success

```

Go to Source
