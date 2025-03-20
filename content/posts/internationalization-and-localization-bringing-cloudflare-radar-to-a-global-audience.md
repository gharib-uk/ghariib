---
title: "Internationalization and localization: bringing Cloudflare Radar to a global audience"
date: 2025-01-02
categories: 
  - "general"
---

Cloudflare Radar celebrated its fourth birthday in September 2024. As we’ve expanded Radar’s scope over the last four years, the value that it provides as a resource for the **global** Internet has grown over time, and with Radar data and graphs often appearing in publications and social media around the world, we knew that we needed to make it available in languages beyond English.

Localization is important because most Internet users do not speak English as a first language. According to W3Techs, English usage on the Internet has dropped 8.3 points (57.7% to 49.4%) since January 2023, whereas usage of other languages like Spanish, German, Japanese, Italian, Portuguese and Dutch is steadily increasing. Furthermore, a CSA Research study determined that 65% of Internet users prefer content in their language.

To successfully (and painlessly) localize any product, it must be internationalized first.  Internationalization is the process of making a product ready to be translated and adapted into multiple languages and cultures, and it sets the foundation to enable your product to be localized later on at a much faster pace (and at a lower cost, both in time and budget). Below, we review how Cloudflare’s Radar and Globalization teams worked together to deliver a Radar experience spanning twelve languages.

## What is localization?

Localization (l10n) is the process of adapting content for a region, including translation, associated imagery, and cultural elements that influence how your content will be perceived. The goal, ideally, is to make the content sound like it was originally written with the region in mind, incorporating relevant cultural nuances instead of merely replacing English with translated text.

Localization includes, among others:

- **Language**: Translation, obviously, but it’s just the beginning.
    
- **Tone and message**: Localization considers what will resonate with your target audience, not just what’s accurate.
    
- **Images**: What may be appropriate in one country can be problematic in another (maps, for instance, that tend to include disputed territories). 
    
- **Date, time, measurement, and number formats**: Formats change based on location and may differ even within the same language. In the U.S., the date follows this format: “December 15, 2018.” But in the U.K., that same date would be written like this: “15 December 2018.” Not to mention a constant source of confusion: the month/day/year vs.day/month/year difference:
    

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/ChruPHkbCpKkaDNfnd3wK/13a299038c2fc70e71fc7e4e71fb8671/image9.png)

_Image: XKCD,_ _https://xkcd.com/2562/_

Pixar movies are a great example of localization**_._** Pixar takes great care to internationalize their movie production process, so they can replace or insert scenes that will resonate with watchers all over the world, not just the US. Let’s consider _Inside Out_ (2015). During the movie, Riley reminisces about playing ice hockey back in Minnesota. Most of the world is not as familiar with ice hockey as in the US, so Pixar wisely decided that they would use soccer elsewhere, allowing a more direct emotional connection with those audiences.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2vmuz0H3YXa6sAh0EJpj0E/5ed37b80f24932c6082a079a587c1ac4/Screenshot_2024-12-14_at_16.15.15.png)

_Images: scene from Inside Out (2015), produced by Pixar Animation Studios and Walt Disney Pictures. Copyright Pixar Animation Studios and Walt Disney Pictures. Images used under fair use._

And you don’t have to go to computer animated movies. Here’s an example from The Shining (1980) where the famous “All work and no play makes Jack a dull boy” typewriter scene was localized into all languages differently. _The producers, in a pre-Information Technology example of internationalization, shot and cut the localized scene into the local versions of the movie._

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3GNqgdcxlk9fLll5OQgilJ/5bfe683547230280ea84741a7c7c0405/Screenshot_2024-12-14_at_16.16.28.png)

_Images: scene from The Shining (1980), directed by Stanley Kubrick. Copyright Warner Bros. Pictures. Images used under fair use._

## Internationalization

Localization is hard, and no one in the business will tell you otherwise. Fortunately there’s a playbook: the first step to localization is **internationalization** (i18n). **Internationalization** is the process of making a product ready to be translated and adapted into multiple languages and cultures. It's a preparatory step that helps with translation and localization. The more you internationalize your code and the more you take into account language and cultural nuances, the easier the localization will be.

### Hard-coding and externalization

The first step to internationalize Radar was to assess how many of the localizable strings were hard-coded. Hard coding is the practice of embedding data directly into the source code of a program. Although a convenient and fast way to write your code, it makes it more difficult to change or localize the code later.

Most of the strings that make up the Radar pages used to be hard-coded, so before we could begin translating, **externalization** had to be done, which is the process of extracting any text that needs to be localized from the code and moving it into separate files.

Hard-coded strings:

```
import Card from “~/components/Card”;
import Chart from “~/components/Chart”;

export default function TrafficChart() {
  return (
    <Card
      title="Traffic"
      description="Share of HTTP requests"
    >
      <Chart />
    </Card>
  );
}
```

Externalized key placeholders:

```
import { useTranslation } from "react-i18next";
import Card from “~/components/Card”;
import Chart from “~/components/Chart”;

export default function TrafficChart() {
  const { t } = useTranslation();
  return (
    <Card
      title={t("traffic.chart.title")}
      description={t("traffic.chart.description")}
    >
      <Chart />
    </Card>
  );
}
```

There are several benefits to externalizing strings:

- It allows translators to work on separate, isolated files that contain only localizable strings 
    
- It prevents accidental changes to the code 
    
- It allows developers to deploy updates, changes, and fixes without having to recompile or redeploy code for each language every time
    

If you look at the example below, when the code is compiled or deployed, upon reaching line 10 (on the left), it will find a key named `traffic.chart.title`. It will then proceed to match that key within the JSON file on the right, finding it on line 1090 and resolving it to “`Traffic`” for English, "`Tráfego`" for Portuguese and "`トラフィック`" for Japanese, doing this for every localized JSON file present in the code.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/14pNiwO9UTdrwbFzq2VUvi/c1a163bc98915195a0cb8ccb29639365/image1.png)

### Pseudo translation

Not all strings are easily found and some are buried deep in the code, sometimes in legacy, inherited code or APIs. Fortunately, there are some strategies that help detect hard-coded strings. This is where **_pseudo translation_** comes into play.

_Pseudo translation_ is a process that replaces all characters in a string with similar-looking ones; pseudo translated strings are enclosed within \[ \] characters, and some extra characters are added to them to simulate text expansion (more on that later). It is an invaluable tool to help us find any hard coded strings, and to stress test the UI for language readiness and length variability, while still keeping the content mostly readable. For example, this string:

`Routing Information`

looks like this once pseudo translated:

`[R~óútíñg Í~ñfó~rmát~íóñ]`

Once pseudo translation is done, any English strings left intact are most likely hard coded or come from other sources. In the screenshot below you can see how `ASN`, `Country`, `Name` and `Prefix Count` did not get pseudo translated and had to be externalized by the Radar developers. The Globalization team collaborated with the Radar team to report and fix hard-coded text issues, as well as the issues that are mentioned in the next few sections.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/16SjYR099rxBLw0YuptB9n/7ca430aa1544349c3e2eff45cdbdfe60/image4.png)

### Text expansion

Text expansion occurs when translated content from one language to another takes up more space than the original. Sometimes this expansion is horizontal, as English to German can expand up to an average of 35%, Spanish 30%, and French 20%). Asian languages might contract from the English but expand vertically. Interestingly, the fewer characters English has, the more the localized languages tend to expand.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3P2FszOadUDhhc1oRzjjra/250d2425f09423907359914bf628e29c/image10.png)

_Data source:_ _IBM_

UI designers and developers need to keep this in mind when creating their applications. Thus, one important consideration is to test the design mock-ups with larger texts and plan the UI to accommodate for text expansion. If some English content barely fits within its container, it will most likely not fit in other languages and possibly break the layout.

Here’s an example of the same button in different languages in Radar’s fixed-width sidebar. Since it’s the main navigation, truncating the text is not appropriate and the only viable option is wrapping, which means localized buttons can end up having different heights. Sometimes it’s necessary to trade visual consistency for usability.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/oByQcEAMDwh4gv1GtxOQG/0de2878728f293a49e03a9b90fbae536/image3.png)

### String concatenation

In English, you can easily chain-connect words because most words lack inflections. Almost all programming languages are designed using the English language in mind. An old linguist joke goes like: _an English teacher: a teacher of English or a teacher from England?_ Case in point, it would be nightmarish to translate this example:

`A lovely little old rectangular green French silver whittling knife` 

Most Western languages need to connect words with some _glue_: prepositions, articles, or inflections. This is why, in general, string concatenation (putting together sentences or sentence parts by combining two or more strings) is a terrible practice for localization, even though it seems efficient from a development point of view. You can’t assume that all languages follow the same sentence structure as English. Most languages don’t.

Sentences may need to be completely reversed for them to sound grammatically correct in other languages. This becomes a particularly severe problem when a string doesn’t include a placeholder because it’s assumed to be concatenated at the beginning or the end of the string, such as this:

`"is currently categorized as:"`

Developers need to make sure to include any placeholders within the string itself, so that translators can easily move them as needed, for instance:

`"Distribution of {{botClass}} traffic by IP version"`

would look like this in Simplified Chinese (notice how the `{{botClass}}` placeholder got moved)

`"{{botClass}} 流量分布（按 IP 版本）"`

### String reuse

As with string concatenation, string reuse (using the same string in more than one place and just swapping out the contents of a placeholder) seems efficient if you’re a developer. A problem arises when translating this into gendered languages, such as most European languages. In Spanish, depending on its position and context, a word as simple as “open” standing by itself, could have all these different translations:

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3rcU3cH7ZXfsn5bxmx11UG/60e9ebfbec19caed3553e8d3b63828fd/image7.png)

Other examples are `Custom`, `Detected`, or `Disabled`, which can have different translations depending on their position within a sentence, their location in the UI, depending on whether they accompany a singular, plural, masculine or feminine noun, so extra entries for these may need to be created in the language files.

Translators will also need to know what will replace the placeholders in strings like the one below, because the surrounding wording may refer to a term that is masculine, feminine, or neutral (for languages that have those, such as German). If a placeholder could be more than one of these (a masculine noun but also a feminine noun), the translation will become grammatically incorrect in at least some of the cases. In the following example, translators would need to know what `{link1}` and `{link2}` will be replaced with, so they know which grammatically correct wording to use around them.

`Your use of the URL Scanner is subject to our {{link1}}. Any personal data in a submitted URL will be handled in accordance with our {{link2}}`.

A better way to do this is to have component placeholders and include the text to be translated for context:

`Your use of the URL Scanner is subject to our <link1>Online Service Terms of Use</link1>. Any personal data in a submitted URL will be handled in accordance with our <link2>Privacy Policy</link2>`.

## Regional considerations

### Date formats

Date formats vary greatly from country to country. Not only can’t you assume that all countries use a month/day/year format, but even the day that the week starts may be different based on the country or culture.

Here’s a comparison of Radar’s date picker in American English against European Spanish (which has weeks starting on Mondays instead of Sundays), and against Simplified Chinese (which uses a completely different format for dates).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/10dcc7wLFMwNmIeMtHeezb/863c9e61d174f9f647379a146b0d4912/image2.png)

Thankfully, developers don’t need to know all the country-specific details, as they can use Intl.DateTimeFormat or Date.toLocaleString() for this.

Intl.DateTimeFormat receives a locale and formatting options that differ from string tokens commonly found on date libraries such as Day.js or Moment.js. Unless you specifically use the localized string tokens on those libraries, the order of the tokens is fixed, along with any characters or delimiters you might add to the format, which poses a problem because the date format parts order should change according to the locale.

Intl.DateTimeFormat handles all that and saves you the trouble of having to add a date formatting dependency to your project and loading library-specific locale resources.

Here’s an example of a generic React component using Intl.DateTimeFormat and react-i18next. The code below will render the date as “Tue, Oct 1, 2024” for American English (en-US) and as “2024年10月1日(火)” for Japanese (ja-JP).

```
import { useTranslation } from "react-i18next";

export default function SomeComponent() {
  const { i18n } = useTranslation();
  const date = new Date("2024-10-01");
  return (
    <time dateTime={date.toISOString()}>
      {new Intl.DateTimeFormat(i18n.language, {
        weekday: "short",
        month: "short",
        year: "numeric",
        day: "numeric",
      }).format(date)}
    </time>
  );
}
```

### Number notations

Similarly, different locales use different notations for numbers. In the US and the UK, a period is used as the decimal separator, and a comma as the thousands separator. Instead, other countries use a comma as the decimal separator and a period (or a space) as the thousands separator. Again, it’s not necessary for developers to know all the odds and ends for this, as they can use Intl.NumberFormat.

Here’s an example of a generic React component using Intl.NumberFormat and react-i18next. The code below will render the number as “12,345,678.90” for American English (en-US) and as “12 345 678,90” for Portuguese (pt-PT). Intl.NumberFormat options can be passed to format numbers as decimals, percentages, currencies, etc, and specify things like number of decimal places and rounding strategies.

```
import { useTranslation } from "react-i18next";

export default function SomeComponent() {
  const { i18n } = useTranslation();
  return (
    <span>{new Intl.NumberFormat(i18n.language, {
    style: "decimal",
    minimumFractionDigits: 2,
  }).format(12345678.9)}</span>);
}
```

As of mid-December, regionalized number formatting is not fully implemented on Radar. We expect this to be complete by the end of Q1 2025.

### List sorting

When you have a list of items that appears sorted, such as a country list in a dropdown, it’s not enough to simply translate the items. For instance, when translated into Portuguese, “`South Africa`” becomes “`África do Sul`”, which means it should then go near the top of the list. Besides that, each language has different sorting requirements, and those go way beyond the A-Z alphabet. For instance, several Asian languages don’t use Latin characters at all, and may get sorted by stroke or character radical order instead.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5uOOTu2U3txnxjcVkhI4VI/37af8b29d9537fc56c4c5e23e3504640/image13.png)

Here’s an example of a generic React country selector component using String.localeCompare and react-i18next. The code below imports a list of countries with name and alpha-2 code and sorts the options according to the translated country name for the active locale. Intl.Collator options can be passed to localeCompare() for specific sorting needs.

```
import { useTranslation } from "react-i18next";

import Select from "~/components/Select";
import COUNTRIES from "~/constants/geo";

export default function CountrySelector() {
  const { t, i18n } = useTranslation();
  const options = COUNTRIES.map(({ name, code }) => ({
    label: t(name, { ns: "countries" }),
    value: code,
  })).sort((a, b) => a.label.localeCompare(b.label, i18n.language));
  return <Select options={options} onChange={(option) => { /* do something */ }} />;
}
```

## API localization

Many of the Radar screens and reports include output from Cloudflare or third-party APIs. Unfortunately, the vast majority of these APIs only output English content. When combining that with the translated part of the site, it may give the impression of a poorly localized site.

To solve this, we take API outputs and map everything into separate files, translate all possible messages, and then display that instead of the original output. But as APIs evolve over time and new messages are added, or existing ones get changed, keeping up with these translations becomes an endless game of “whac-a-mole".

```
"Address unreachable error when attempting to load page": "Error de dirección inaccesible al intentar cargar la página",
"Authentication failed": "Autenticación fallida",
"Browser did not fully start before timeout": "El navegador no se ha iniciado por completo antes de agotar el tiempo de espera.",
"Certificate and/or SSL error when attempting to load page": "Error de certificado y/o SSL al intentar cargar la página",
"Crawl took too long to finish": "El rastreo ha tardado demasiado en completarse.",
"DNS resolution failed": "Error de resolución de DNS",
"Network connection aborted.": "Conexión de red cancelada"
```

It should be a best practice for APIs to accept a locale parameter or header, and for engineers to have multiple languages in mind when building these APIs, even if it’s just the error messages. That could save time and resources for any number of clients they might have.

## Project setup

Radar is a Remix project running on Cloudflare Pages. While researching ways to implement internationalization, we came across this Remix blog post, and after some experimenting, we decided to go with Sergio Xalambrí’s remix-i18next. We mostly followed the installation instructions found on the repo, with some changes.

We have multiple translation files on every locale folder, one for each data source, to help us maintain strings that come from APIs. Each file can be used to create a namespace for translations, to avoid key collisions, and also to be loaded separately as needed for each route or component.

On remix-i18next’s instructions, you’ll find the concept of backend plugins to achieve the loading of these files, except that you cannot use i18next-fs-backend with Cloudflare Pages because there’s no access to the filesystem. To solve that we used the _resources_ approach, similar to what can be found on this sample remix-i18next with vite setup, but we didn’t want to have to maintain the resources dictionary each time we add new namespaces, so vite’s Glob imports came in handy:

```
import { serverOnly$ } from "vite-env-only/macros";

export const resources = serverOnly$(
  Object.entries(import.meta.glob("./*/*.json", { import: "default" })).reduce(
    (acc, [path, module]) => {
      const parts = path.split("/").slice(1);
      const locale = parts[0];
      const namespace = parts[1].split(".")[0];
      if (!acc[locale]) acc[locale] = {};
      if (!acc[locale][namespace]) acc[locale][namespace] = {};
      module().then((value) => (acc[locale][namespace] = value));
      return acc;
    },
    {},
  ),
)!;
```

This creates a server-side only resources dictionary by importing all JSON files in the locales folder to be passed as the resources property for the i18next configuration in Remix’s entry.server.tsx.

To load namespaces on the client side, we created a Remix resource route that uses the resources dictionary and responds with the namespace object of the requested locale:

```
import { LoaderFunctionArgs } from "@remix-run/server-runtime";

import { resources } from "~/i18n";

export async function loader({ params }: LoaderFunctionArgs) {
  const { locale, namespace } = params;
  return resources[locale]?.[namespace] || {};
}
```

You can then use the i18next-http-backend or i18next-fetch-backend backend plugins for the i18next configuration in Remix’s entry.client.tsx:

```
import i18next from "i18next";
import i18nextHttpBackend from "i18next-http-backend";
import { initReactI18next } from "react-i18next";
import { getInitialNamespaces } from "remix-i18next/client";
import { options } from “~/i18n”;
  
await i18next
    .use(initReactI18next)
    .use(i18nextHttpBackend)
    .init({
      ...options,
      ns: getInitialNamespaces(),
      backend: { loadPath: "/api/i18n/{{lng}}/{{ns}}" },
    });
```

Default namespaces are defined with the _defaultNS_ config property:

```
export const defaultNS = ["main", "countries"];
```

Additional namespaces to be used for each route can be defined through Remix’s handle export:

```
export const handle = {
  i18n: ["url-scanner", "domain-categories"],
};
```

Namespaces on the server side get picked up by the _getRouteNamespaces_ function on entry.server.tsx:

```
const ns = i18n.getRouteNamespaces(remixContext);
```

On the client-side, examples suggested that you’d have to declare the namespaces on each _useTranslation()_ hook instance, but we worked around that in Remix’s root.tsx file:

```
import { useLocation, useMatches } from "@remix-run/react";
import { useTranslation } from "react-i18next";
import { defaultNS } from “~/i18n”;

export function Layout({ children }) {
  const location = useLocation();
  const matches = useMatches();
  const handle = matches?.find((m) => m.pathname === location.pathname)?.handle || {};
  useTranslation([...new Set([...defaultNS, ...(handle.i18n || [])])]);
  ...
}
```

This causes the client-side plugin to make calls to the resource route and load the required namespaces.

We also wanted to have the locale in the URL pathname, but not for the default language, so Remix’s optional segments allowed us to do just that. remix-i18next does not have URL locale detection by default, but you can provide your own findLocale function that will receive the request as an argument, and you can then parse the request URL to extract the locale.

## Search engine optimization

Once you set up your project for internationalization, you can inform search engines of localized versions of your pages. This allows search engines to display localized results of your website in the same language that is being searched.

```
<head>
  ...
  <link rel="alternate" href="https://radar.cloudflare.com/" hreflang="en-US">
  <link rel="alternate" href="https://radar.cloudflare.com/de-de" hreflang="de-DE">
  <link rel="alternate" href="https://radar.cloudflare.com/es-es" hreflang="es-ES">
  <link rel="alternate" href="https://radar.cloudflare.com/es-la" hreflang="es-LA">
  <link rel="alternate" href="https://radar.cloudflare.com/fr-fr" hreflang="fr-FR">
  <link rel="alternate" href="https://radar.cloudflare.com/it-it" hreflang="it-IT">
  <link rel="alternate" href="https://radar.cloudflare.com/ja-jp" hreflang="ja-JP">
  <link rel="alternate" href="https://radar.cloudflare.com/ko-kr" hreflang="ko-KR">
  <link rel="alternate" href="https://radar.cloudflare.com/pt-br" hreflang="pt-BR">
  <link rel="alternate" href="https://radar.cloudflare.com/pt-pt" hreflang="pt-PT">
  <link rel="alternate" href="https://radar.cloudflare.com/zh-cn" hreflang="zh-CN">
  <link rel="alternate" href="https://radar.cloudflare.com/zh-tw" hreflang="zh-TW">
  <link rel="alternate" href="https://radar.cloudflare.com/" hreflang="x-default">
  <link rel="canonical" href="https://radar.cloudflare.com/">
  ...
</head>
```

You should also localize page titles, descriptions, and relevant Open Graph metadata. To achieve this with Remix and remix-i18next, you use the getFixedT method in route loaders to resolve the translations and return data for the meta export:

```
import type { LoaderFunctionArgs, MetaArgs } from "@remix-run/server-runtime";
import i18n from "~/i18next.server";

export async function loader({ request }: LoaderFunctionArgs) {
  const t = await i18n.getFixedT(request);
  return {
    meta: {
      title: t("meta.about.title"),
      description: t("meta.about.description"),
      url: request.url,
    },
  };
}

export const meta = ({ data }: MetaArgs<typeof loader>) => data.meta;
```

If you are defining default meta tags in parent routes you may also need to merge the meta objects.

## Conclusion

There is hardly ever an absolute when dealing with languages. Your Spanish, your French, your Japanese will be different from someone else’s, even if you grew up next door to each other. Family, education, environment, relationships will season and give color to your language. It is like a family recipe — it’s unique, it feels like home and it’s not negotiable. It does not make it better or worse, it just makes it yours. And yours will always be different from other languages.

Localization is hard. We have seen that there are many things that can and will go sideways, and there are many unknowns that bubble up to the surface in the process. It can also make a product better, as it stress tests the product’s code and design. A tight relationship between the globalization and Radar teams helped make our efforts go more smoothly. In addition, our translators stepped up to the challenge, familiarizing themselves with Radar, analyzing the English content, finding the right translation that will not only resonate with the audience but also fit in the space allotted, constantly checking for context, previous translations, consistency, industry standards, adapting to style guides, tone, and messaging, and after all of that, ultimately acknowledging the fact that there will be people who will disagree (to varying levels of zeal) with their choice of words.

If you haven’t done so already, we encourage you to explore the localized versions of Cloudflare Radar. Click the language drop-down in the upper right corner of the Radar interface and select your language of choice — Radar will be presented in that language until a new selection is made. Have comments or suggestions about the translations? Let us know at radar\_localization@cloudflare.com.

Go to Source
