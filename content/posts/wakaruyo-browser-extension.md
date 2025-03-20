---
title: "Wakaruyo - Browser Extension"
date: 2025-01-08
tags: 
  - "browserextension"
  - "japanese"
  - "tech"
---

Hello everyone!  
  
Those who know me are aware of how much I love the Japanese language and how dedicated I am to self-studying it. They also know that I can never sit still. For this reason, I developed **Wakaruyo**, a browser extension that is currently in the experimental phase.

This extension allows you to translate selected text on a webpage from Japanese to the language set in your browser, which is automatically detected.

![Wakaruyo](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fn86qbgvbigm87pdqtnyp.png)

## Main features include:

### 1\. Text Selection and Translation

When a user selects text on a webpage, the extension automatically detects if the text is in Japanese and initiates a translation request via API. The translation is then displayed in a floating "overcard" next to the selected text. The functionality also supports Japanese text within images.

### 2\. Translation Caching

Translations are stored locally in the browser to reduce API calls and speed up repeated translations of the same text. The cache is managed through `localStorage` and has an expiration time to ensure continuous updates.

### 3\. Retry Mechanism

If an API request fails due to network issues or other reasons, the extension will automatically retry up to three times, with a delay between each attempt.

### 4\. Romanization Support

The extension can also convert Japanese text into **Romanji** (the transcription of Japanese into the Latin alphabet). Users can easily toggle between the translation and Romanji with a simple click.

### 5\. Translation Requests with Debounce

To prevent overloading the API with too many requests, the extension uses a **debounce** function that only sends the translation request after the user has finished selecting the text, with a short delay.

Video preview on X.com:  
  

>   
> 
> 🚀 I created Wakaruyo, a browser extension to automatically translate Japanese! Select text on a webpage and instantly get the translation in your browser's language. Supports romanization, cache, retry, and more! 🌐 #Tech #Japanese #BrowserExtension pic.twitter.com/tSwpKDpY53
> 
> — Matteo Santoro Dev (@matteosant\_dev) January 8, 2025  

Go to Source
