---
title: "FindSecBugs officially an OWASP project"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "findsecbugs"
  - "forensics"
  - "java"
  - "owasp"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
---

Over the years, Find Security Bugs _– or FindSecBugs in short –_ has evolved from a limited static-analysis tool to one with solid coverage of bug patterns. In this post, we will present the latest milestone from the project: arrival in the OWASP family, some figures and details regarding its new release.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguRN7IP7h0r2NCfuHqVv07ubueit4pTin2L1enbcKvxvZztvyqoM-aNNQup_GNQ8aVtH_S9QpEfwx534PAruhv3R3xnWv5g9lBah20tkzyMreFmlAVKPbv34BaoGRyf8LMQs1LjJER91hj/s16000/fsb%255B1%255D.png)

  

## Joining the OWASP foundation

The main motivation for joining the OWASP foundation is to make it clear that the project is a community effort. While GoSecure is using the tool internally for code review assessments, it does not plan to commercialize this specific product. Under the OWASP umbrella, it should be clear to the future contributors that the project is not owned by a single organization or author.

  
Getting new and active contributors is one of the big long-term challenges for an open-source project. At the moment, the number of external contributions is steady and of quality. This is still something to continue to watch to assure long-term stability.

## Moving forward

Becoming an OWASP project is not an the end in itself. We still have plenty of new bug patterns to identify. One of our goals is to reach **2 to 3 releases a year**. To provide this release cycle, more time would need to be attributed to the development of new features, but also to **improve the integration testing phase**. While unit tests cover the functional aspect well, the overall performance and different integration tests need to be done manually at the present time. Being an open project, we will continue to improve the developer documentation to make contributions straightforward.

  

## Some Numbers

**50** total contributors, **10** active this year

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgHjkiXwCGJ_uqC6ZwyH0yH5gKOlwgA6qc20E4F7MWZsYiAdvNQqRzZF04JFXSjB2KbMlkRPYc24TE-dta8gzvgWMglUfGevx-NANkpEa-Waupqb_9L3vT3bPY7vEYKIY16ExcGtXBzqLpJ/w640-h160/2019-10-07-12_37_05-Contributors-to-find-sec-bugs_find-sec-bugs%255B1%255D.png)

  

  

More than **1100** commits over the past **7** years

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgEDtNzNOxNXpwfs1foW0KT_XvCOww9GSU7N1N2vgAfujInsSLe_o4pcANA1U9O6zi_mjkWUxt5XOx9UaAE8mVVH1vrK_g5mH35Qz24iLgH6SZjGFg6-q7dbvmSNmNL1hMvpCDzlvrijK0h/w640-h166/commits%255B1%255D.png)

**208k** downloads for the past 12 months (Source: Sonatype)

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhl_m6SBDOGcnqe0hLtrGwTRLJazXfQz2wqZl0T0UwEr1A-QYQlHT4KwcO7VFS4FN4E2Xgiu0QJx2ihYqqs9F_ajMvikEblGuppct6V587xJFdLxbW3ljbzgSg9yGIZMmpTG8Z94P8cxtNE/w640-h160/maven_central_chart%255B1%255D.png)

There are **300** units tests with **84%** coverage.

_We are looking to improve the coverage to 90%_

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjgsRaRT18A1t3J0rJWq-aNsu3QFGYY6hAOnQn-b8Q_imWH_js_niami7DXQ4RzKKKth4z04gzu09dUhcscaRVRVMW7A3qhZmkjY_EEpnfEfAw-NSlO3og8yomONFlWeMD1rceg07RcS79P/w640-h138/2019-10-07-12_52_41-https___codecov.io_gh_find-sec-bugs_find-sec-bugs_commit_e3d890aefd70b5fd9897795%255B1%255D.png)

  

## New vulnerability detectors in FSB 1.10.0

A new version will be released this week. With this release comes some bug fixes and improvements to existing bug detectors. There are also a few additions that are likely to find new vulnerability classes in your code base.

### New bug detectors (or important improvements)

- Mass-assignment when using JPA or JDO entities
- Leakage from entity when using JPA or JDO entities
- Permissive CORS header allowing all origin (New coverage for Spring CorsRegistry)
- Overly permissive file permissions (code doing equivalent operation to chmod 777)
- Insecure SAML configuration affecting provider using OpenSAML API

You can view the complete list of bug patterns currently supported on the website.

### Improving FindSecBugs beyond Spotbugs

The SpotBugs integration is critical to the user experience of Find Security Bugs. We are planning to make improvements to the IDE plugins (IntelliJ and Eclipse). We will be looking at language support such as Kotlin and JSP. At the moment, the IDE plugins only support Java source code correctly.

An example of an upcoming contribution to SpotBugs integration is the enhancements of the Jenkins Warning plugin to support any languages not just Java. This change will also be benefiting other static code analysis tools such as Brakeman. The new code highlighter (Prism.js) is displayed below.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgGLgrCMH3yC-JnrhjDwwthHHc3pQEYEPDFhfMM_UuJwDDVrWFTqrYHPSNP04XaNlY9mFOI29S_aPKUpYIHyK4G629IaCkKV-6Jyj44iAEt0PuRQQ9fCl99svJlJEAywiOKOBTfh2bOUpw3/w640-h320/image31%255B1%255D.png)

## Hacktoberfest is coming

If you are an existing user and would like to contribute to the project, there is no better time than the Hacktoberfest. The Hacktoberfest is taking place this month. Multiple issues were tagged in the issue tracker with the tag **\[hacktoberfest\]**. Those issue are easy to complete for newcomers. Don’t hesitate to communicate your interest in contributing on the GitHub bug tracker.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhC8CUcEFp4GmR1TVdrgGNGQVqiAz8opeE1vVVyMA5dOYfLXbYz-DdeAIP1d9a2sVjLKfOhjtiHEkhBRyZ5J2o1brN3mm4aG6TmWyK9g90NBWlSQUnkQoIv6goxhYJbFDkpo9JV5dpgNdGJ/w640-h320/hackto%255B2%255D.jpg)

  

  

That’s all folks, until next blog happy code review !

Go to Source
