---
title: "Vertex AI Studio data exfiltration"
date: Thu, 19 Oct 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Vertex AI Studio data exfiltration

<br/>

<br/>
In Vertex AI Studio, a Prompt Injection attack could cause the LLM to return markdown tags. This could have allowed an adversary whose data makes it into the chat context (e.g., via an uploaded file) to achieve exfiltration of the victim’s data by rendering hyperlinks. However, the severity of this issue is low, as there were no integrations that could pull remote content. This means Indirect Prompt Injection was not possible, and it would require the victim to copy the malicious prompt from elsewhere. A similar issue affected Azure AI.

#### [Source](https://www.cloudvulndb.org/gcp-vertex-ai-data-exfil)

<br/>
---
