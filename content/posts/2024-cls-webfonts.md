---
title: "<div>Fixing layout shifts caused by web fonts</div>"
date: 2025-03-19
categories: 
  - "general"
---

In 2020, Google introduced Core Web Vitals metrics to measure some aspects of real-world user experience on the web. This blog has consistently achieved good scores for two of these metrics: Largest Contentful Paint and Interaction to Next Paint. However, optimizing the third metric, Cumulative Layout Shift, which measures unexpected layout changes, has been more challenging. Let’s face it: optimizing for this metric is not really useful for a site like this one. But getting a better score is always a good distraction. 💯

To prevent the “flash of invisible text” when using web fonts, developers should set the `font-display` property to `swap` in `@font-face` rules. This method allows browsers to initially render text using a fallback font, then replace it with the web font after loading. While this improves the LCP score, it causes content reflow and layout shifts if the fallback and web fonts are not metrically compatible. These shifts negatively affect the CLS score. CSS provides properties to address this issue by overriding font metrics when using fallback fonts: `size-adjust`, `ascent-override`, `descent-override`, and `line-gap-override`.

Two comprehensive articles explain each property and their computation methods in detail: Creating Perfect Font Fallbacks in CSS and Improved font fallbacks.

# Interactive tuning tool

Instead of computing each property from font average metrics, I put together a tool for interactively tuning fallback fonts.1

## Instructions

1. Load your custom font.
    
2. Select a fallback font to tune.
    
3. Adjust the `size-adjust` property to match the width of your custom font with the fallback font. With a proportional font, it is not possible to achieve a perfect match.
    
4. Fine-tune the `ascent-override` property. Aim to align the final dot of the last paragraph while monitoring the font’s baseline. For more precise adjustment, disable the “Set line height” option.
    
5. Modify the `descent-override` property. The goal is to make the two boxes match. You may need to alternate between this and the previous property for optimal results.
    
6. If necessary, adjust the `line-gap-override` property. This step is typically not required.
    

The process needs to be repeated for each fallback font. Some platforms may not include certain fonts. Notably, Android lacks most fonts found in other operating systems. It replaces _Georgia_ with _Noto Serif_, which is not metrically-compatible.

## Tool

This tool is not available from the Atom feed.

## Results

For the body text of this blog, I get the following CSS definition:

```
@font-face {
  font-family: Merriweather;
  font-style: normal;
  font-weight: 400;
  src: url("../fonts/merriweather.woff2") format("woff2");
  font-display: swap;
}
@font-face {
  font-family: "Fallback for Merriweather";
  src: local("Noto Serif"), local("Droid Serif");
  size-adjust: 98.3%;
  ascent-override: 99%;
  descent-override: 27%;
}
@font-face {
  font-family: "Fallback for Merriweather";
  src: local("Georgia");
  size-adjust: 106%;
  ascent-override: 90.4%;
  descent-override: 27.3%;
}

font-family: Merriweather, "Fallback for Merriweather", serif;

```

After a month, the CLS metric improved to 0:

<figure>

![Core Web Vitals scores for vincent.bernat.ch showing all 6 metrics as green.
Notably the Cumulative Layout Shift is 0.](https://d2pzklc15kok91.cloudfront.net/images/core-web-vitals.d7a1858e4d5b4b.svg)

<figcaption>

Recent Core Web Vitals scores for vincent.bernat.ch

</figcaption>

</figure>

# About custom fonts

Using safe web fonts or a modern font stack is often simpler. However, I prefer custom web fonts. Merriweather and Iosevka, which are used in this blog, enhance the reading experience. An alternative approach could be to use _Georgia_ as a serif option. Unfortunately, most default monospace fonts are ugly.

Furthermore, paragraphs that combine proportional and monospace fonts can create visual disruption. This occurs due to mismatched vertical metrics or weights. To address this issue, I adjust Iosevka’s metrics and weight to align with Merriweather’s characteristics.

* * *

1. Similar tools already exist, like the Fallback Font Generator, but they were missing a few features, such as the ability to load the fallback font or to have decimals for the CSS properties. And no source code. ↩︎
