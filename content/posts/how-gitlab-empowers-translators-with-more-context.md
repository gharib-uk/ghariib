---
title: "<div>How GitLab empowers translators with more context</div>"
date: 2025-01-03
categories: 
  - "general"
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

GitLab is continuously translated by our global community of translators and proofreaders. Through Crowdin, they help make our product more accessible to the world by translating it into 78 languages. GitLab translation community members are volunteer translators, working professionals who are GitLab users, and even university students contributing to translators as part of their classroom projects.

### How the GitLab open-core model supports translators

While this community collaboration is powerful, translators often face a crucial challenge: understanding the full context of the text that they are translating.

Great translation isn't just about converting words – it's about preserving meaning, intent, and usability in a target language. Software translation requires a unique blend of skills. Great translators usually specialize in multiple linguistic domains as well as the technical domain of the product itself. They perform myriad translation-adjacent tasks such as:

- ensuring accuracy and consistency of technical terminology
- creating new glossary terms for new concepts
- adhering to the style and tone
- navigating the complexity of CLDR pluralization rules
- understanding the translatability of composite messages and how to provide feedback to improve the source by making it localizable

Translators spend lots of time researching context, asking questions, and reading GitLab product documentation. Without proper context, even simple phrases can be mistranslated, potentially confusing or disrupting user workflows. What sets GitLab apart is its open-core model, which allows translators to access most of the product development context directly at the source. This transparency empowers them to actively contribute as true co-creators of the GitLab global product.

### Introducing our new context-enhancement feature

To support these needs and empower translators with context, GitLab has developed a new feature: we now embed into each translatable string a contextual link (more specifically, a link to our global in-product search) that lets translators locate all instances of that string in the codebase. These links allow translators to rely on Git blame to trace every string's history – from its current location in code, through commits in merge requests, and all the way up to the original planning discussions.

For example, when you are translating **Rotate**, the **AccessTokens|** namespace suggests that the context is, well, _access tokens_. However, there is no additional visual context for what **Rotate** means, and translators are left to wonder, guess, and provide the best possible assumption in a target language.

With the new context enhancement, here is what translators are now able to do:

1. Click the URL in the **See where this string is used in code** section.

![see where this string is used in code section](https://images.ctfassets.net/r9o86ar0p03f/51sveAEk34z4ZAXPmF4KE6/61f6a98458785a4151599be5a3fb327f/image3.png)

2. Review the result of the code search, and click the **View blame** icon:

![Screen with View blame icon](https://images.ctfassets.net/r9o86ar0p03f/5GTN8p8UyotWv1297Fz20i/5f9a1fc0cdacbc1462de08202b04c29c/image6.png)

3. Follow the link with the relevant merge request (Introduce rotation of personal tokens in UI):

![Link with relevant merge request](https://images.ctfassets.net/r9o86ar0p03f/5Q7SEN6tDq1JdAoeJ5Ccbe/a521b967ca4fcb35e1292ffd7ec5a6a0/image7.png)

4. On the **Commits** page, follow the link to the actual merge request:

![Commits page with link](https://images.ctfassets.net/r9o86ar0p03f/5pJ05XH1d8GDfEtVsUULQl/2a52e063e3f258d1703c4ec47d544947/image1.png)

5. Watch the screen recording that the software engineer added to the merge request:

![screen recording in merge request](https://images.ctfassets.net/r9o86ar0p03f/4kwm321zND7pnAoWBav2o3/978eb2661c3f85841201fda680f5fb5e/image5.png)

6. Research even further by going to:  
    a. The linked issue Introduce renew expired token capability in UI or its parent epic Rotate Token through the UI:

![linked issue and parent epic](https://images.ctfassets.net/r9o86ar0p03f/7dqX7PAkCtTvK2smxtLkNe/db07008f3045f4f6d6cb168285bf5c9b/image4.png)

b. The related merge request that updates the GitLab product documentation:

![related merge request to update GitLab product documentation](https://images.ctfassets.net/r9o86ar0p03f/Eth5PEDRJLT6boeNmg0wH/98ddeb594acc346611477c6ec54cdb2a/image2.png)

All these research steps will lead translators to better understand the technical concept of _access token rotation_ and why the token rotation functionality was added, how it works, and what problem it solves for users.

With this rich research trail, translators get the maximum amount of context that will help provide the technically accurate and linguistically correct translation for the seemingly simple word **Rotate**.

This approach goes far beyond traditional translation aids like providing screenshots or exploring the product user interface. Translators can now fully understand the context by:

- deriving context from paths and naming conventions used in code
- viewing screenshots or video recordings that are added to original merge requests
- reading original planning and development discussions
- following the engineering, copywriting, and product management decision-making process that has led to specific wording

### More AI-powered contextual features coming up

The GitLab Localization team isn't stopping here. We are working on more context-enhancement features, including AI-powered tools to help translators understand string usage and placement. Imagine a system where translators could interact with an agent by asking questions or proactively receiving immediate code-aware responses about where and how strings are used in the product UI.

> ### Join our community on Crowdin as a translator or a proofreader, try these new context features, and let us know how we can make the translation experience and our product even better.

GitLab is continuously translated by our global community of translators and proofreaders. Through Crowdin, they help make our product more accessible to the world by translating it into 78 languages. GitLab translation community members are volunteer translators, working professionals who are GitLab users, and even university students contributing to translators as part of their classroom projects.

### How the GitLab open-core model supports translators

While this community collaboration is powerful, translators often face a crucial challenge: understanding the full context of the text that they are translating.

Great translation isn't just about converting words – it's about preserving meaning, intent, and usability in a target language. Software translation requires a unique blend of skills. Great translators usually specialize in multiple linguistic domains as well as the technical domain of the product itself. They perform myriad translation-adjacent tasks such as:

- ensuring accuracy and consistency of technical terminology
- creating new glossary terms for new concepts
- adhering to the style and tone
- navigating the complexity of CLDR pluralization rules
- understanding the translatability of composite messages and how to provide feedback to improve the source by making it localizable

Translators spend lots of time researching context, asking questions, and reading GitLab product documentation. Without proper context, even simple phrases can be mistranslated, potentially confusing or disrupting user workflows. What sets GitLab apart is its open-core model, which allows translators to access most of the product development context directly at the source. This transparency empowers them to actively contribute as true co-creators of the GitLab global product.

### Introducing our new context-enhancement feature

To support these needs and empower translators with context, GitLab has developed a new feature: we now embed into each translatable string a contextual link (more specifically, a link to our global in-product search) that lets translators locate all instances of that string in the codebase. These links allow translators to rely on Git blame to trace every string's history – from its current location in code, through commits in merge requests, and all the way up to the original planning discussions.

For example, when you are translating **Rotate**, the **AccessTokens|** namespace suggests that the context is, well, _access tokens_. However, there is no additional visual context for what **Rotate** means, and translators are left to wonder, guess, and provide the best possible assumption in a target language.

With the new context enhancement, here is what translators are now able to do:

1. Click the URL in the **See where this string is used in code** section.

![see where this string is used in code section](https://images.ctfassets.net/r9o86ar0p03f/51sveAEk34z4ZAXPmF4KE6/61f6a98458785a4151599be5a3fb327f/image3.png)

2. Review the result of the code search, and click the **View blame** icon:

![Screen with View blame icon](https://images.ctfassets.net/r9o86ar0p03f/5GTN8p8UyotWv1297Fz20i/5f9a1fc0cdacbc1462de08202b04c29c/image6.png)

3. Follow the link with the relevant merge request (Introduce rotation of personal tokens in UI):

![Link with relevant merge request](https://images.ctfassets.net/r9o86ar0p03f/5Q7SEN6tDq1JdAoeJ5Ccbe/a521b967ca4fcb35e1292ffd7ec5a6a0/image7.png)

4. On the **Commits** page, follow the link to the actual merge request:

![Commits page with link](https://images.ctfassets.net/r9o86ar0p03f/5pJ05XH1d8GDfEtVsUULQl/2a52e063e3f258d1703c4ec47d544947/image1.png)

5. Watch the screen recording that the software engineer added to the merge request:

![screen recording in merge request](https://images.ctfassets.net/r9o86ar0p03f/4kwm321zND7pnAoWBav2o3/978eb2661c3f85841201fda680f5fb5e/image5.png)

6. Research even further by going to:  
    a. The linked issue Introduce renew expired token capability in UI or its parent epic Rotate Token through the UI:

![linked issue and parent epic](https://images.ctfassets.net/r9o86ar0p03f/7dqX7PAkCtTvK2smxtLkNe/db07008f3045f4f6d6cb168285bf5c9b/image4.png)

b. The related merge request that updates the GitLab product documentation:

![related merge request to update GitLab product documentation](https://images.ctfassets.net/r9o86ar0p03f/Eth5PEDRJLT6boeNmg0wH/98ddeb594acc346611477c6ec54cdb2a/image2.png)

All these research steps will lead translators to better understand the technical concept of _access token rotation_ and why the token rotation functionality was added, how it works, and what problem it solves for users.

With this rich research trail, translators get the maximum amount of context that will help provide the technically accurate and linguistically correct translation for the seemingly simple word **Rotate**.

This approach goes far beyond traditional translation aids like providing screenshots or exploring the product user interface. Translators can now fully understand the context by:

- deriving context from paths and naming conventions used in code
- viewing screenshots or video recordings that are added to original merge requests
- reading original planning and development discussions
- following the engineering, copywriting, and product management decision-making process that has led to specific wording

### More AI-powered contextual features coming up

The GitLab Localization team isn't stopping here. We are working on more context-enhancement features, including AI-powered tools to help translators understand string usage and placement. Imagine a system where translators could interact with an agent by asking questions or proactively receiving immediate code-aware responses about where and how strings are used in the product UI.

> ### Join our community on Crowdin as a translator or a proofreader, try these new context features, and let us know how we can make the translation experience and our product even better.

Go to Source
