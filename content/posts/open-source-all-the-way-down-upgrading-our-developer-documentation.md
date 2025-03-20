---
title: "Open source all the way down: Upgrading our developer documentation"
date: 2025-01-08
tags: 
  - "innovation"
  - "investigation"
  - "javascript"
  - "tech"
  - "un"
---

At Cloudflare, we treat developer content like a product, where we take the user and their feedback into consideration. We are constantly iterating, testing, analyzing, and refining content. Inspired by agile practices, treating developer content like an open source product means we approach our documentation the same way an open source software project is created and maintained.  Open source documentation empowers the developer community because it allows anyone, anywhere, to contribute content. By making both the content and the framework of the documentation site publicly accessible, we provide developers with the opportunity to not only improve the material itself but also understand and engage with the processes that govern how the documentation is built, approved, and maintained. This transparency fosters collaboration, learning, and innovation, enabling developers to contribute their expertise and learn from others in a shared, open environment. We also provide feedback to other open source products and plugins, giving back to the same community that supports us.

## Building the best open source documentation experience

Great documentation empowers users to be successful with a new product as quickly as possible, showing them how to use the product and describing its benefits. Relevant, timely, and accurate content can save frustration, time, and money. Open source documentation adds a few more benefits, including building inclusive and supportive communities that help reduce the learning curve. We love being open source!

While the Cloudflare content team has scaled to deliver documentation alongside product launches, the open source documentation site itself was not scaling well. developers.cloudflare.com had outgrown the workflow for contributors, plus we were missing out on all the neat stuff created by developers in the community.

Just like a software product evaluation, we reviewed our business needs. We asked ourselves if remaining open source was appropriate? Were there other tools we wanted to use? What benefits did we want to see in a year or in five years? Our biggest limitations in addition to the contributor workflow challenges seemed to be around scalability and high maintenance costs for user experience improvements. 

After compiling our wishlist of new features to implement, we reaffirmed our commitment to open source. We valued the benefit of open source in both the content and the underlying framework of our documentation site. This commitment goes beyond technical considerations, because it's a fundamental aspect of our relationship with our community and our philosophy of transparency and collaboration. While the choice of an open source framework to build the site on might not be visible to many visitors, we recognized its significance for our community of developers and contributors. Our decision-making process was heavily influenced by two primary factors: first, whether the update would enhance the collaborative ecosystem, and second, how it would improve the overall documentation experience. This focus reflects that our open source principles, applied to both content and infrastructure, are essential for fostering innovation, ensuring quality through peer review, and building a more engaged and empowered user community.

## Cloudflare developer documentation: A collaborative open source approach

Cloudflare’s developer documentation is open source on GitHub, with content supporting all of Cloudflare’s products. The underlying documentation engine has gone through a few iterations, with the first version of the site released in 2020. That first version provided dev-friendly features such as dark mode and proper code syntax. 

### 2021 update: enhanced documentation engine

In 2021, we introduced a new custom documentation engine, bringing significant improvements to the Cloudflare content experience. The benefits of the Gatsby to Hugo migration included:

- **Faster development flow**: The development flow replicated production behavior, increasing iteration speed and confidence. Preview links via Cloudflare Pages were also introduced, so the content team and stakeholders could quickly review what content would look like in production.
    
- **Custom components**: Introduced features like resources-by-selector which let us reference content throughout the repository and gave us the flexibility to expand checks and automations.
    
- **Structured changelog management**: Implementation of structured YAML changelog entries which facilitated sharing with various platforms like RSS feeds, Developer Discord, and within the docs themselves.
    
- **Improved performance**: Significant page load time improvements with the migration to HTML-first and almost instantaneous local builds.
    

These features were non-negotiable as part of our evaluation of whether to migrate. We knew that any update to the site had to maintain the functionality we’d established as core parts of the new experience.

### 2024 update: Say “hello, world!” to our new developer documentation, powered by Astro

After careful evaluation, we chose to migrate from Hugo to the Astro (and by extension, JavaScript) ecosystem. Astro fulfilled many items on our wishlist including:

- **Enhanced content organization**: Improved tagging and better cross-referencing of  related pages.
    
- **Extensibility**: Support for user plugins like starlight-image-zoom for lightbox functionality.
    
- **Development experience**: Type-checking at build time with astro check, along with syntax highlighting, Intellisense, diagnostic messages, and plugins for ESLint, Stylelint, and Prettier. 
    
- **JavaScript/TypeScript support**: Aligned the docs site framework with the preferred languages of many contributors, facilitating easier contribution.
    
- **CSS management**: Introduction of Tailwind and scoped styles.
    
- **Content collections**: Offered various ways to manage and enhance tagging practices including Markdown front matter validated by Zod schemas, JSON schemas for Intellisense, and a JavaScript callback for filtering returned entries.
    

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1wz2uWlAwbHFG4QgG0d8tt/4eeb3fbcd4d9b33c5590be39654bbff1/BLOG-2600_2.png)

Starlight, Astro’s documentation theme, was a key factor in the decision. Its powerful component overrides and plugins system allowed us to leverage built-in components and base styling.

### How we migrated to Astro

Content needed to be migrated quickly. With dozens of pull requests opened and merged each day, entering a code freeze for a week simply wasn’t feasible. This is where the nature of abstract syntax trees (ASTs) came into play, only parsing the structure of a Markdown document rather than details like whitespace or indentation that would make a regular expression approach tricky.

With Hugo in 2021, we configured code block functionality like titles or line highlights with front matter inside the code block.

```
---
title: index.js
highlight: 1
---
const foo = "bar";
```

Starlight uses Expressive Code for code blocks, and these options are now on the opening code fence.

```
js title="index.js" {1}
const foo = "bar";
```

With astray, this is a simple as visiting the \`code\` nodes and:

1. Parsing \`node.value\` with front-matter.
    
2. Assigning the attributes from \`front-matter\` to \`node.meta\`.
    
3. Replacing \`node.value\` with the rest of the code block.
    

```
import { fromMarkdown } from "mdast-util-from-markdown";
import { toMarkdown } from "mdast-util-to-markdown";
 
import * as astray from "astray";
import type * as MDAST from "mdast";
import fm from "front-matter";
 
const markdown = await Bun.file("example.md").text();
 
const AST = fromMarkdown(markdown);
 
astray.walk<MDAST.Root, void, any>(AST, {
    code(node: MDAST.Code) {
        const { attributes, body } = fm(node.value);
        const { title, highlight } = attributes;
 
        if (title) {
            node.meta = `title="${title}"`;
        }
 
        if (highlight) {
            node.meta += ` {${highlight}}`;
        }
 
        node.value = body;
 
        return;
    }
})
```

## The migration in numbers

When we migrated from Gatsby to Hugo in 2021, the pull request included 4,850 files and the migration took close to three weeks from planning to implementation. This time around, the migration was nearly twice as large, with 8,060 files changed. Our planning and migration took six weeks in total:

- 10 days: Evaluate platforms, vendors, and features 
    
- 14 days: Migrate the components required by the documentation site
    
- 5 days: Staging and user acceptance testing (UAT) 
    
- 8 hours: Code freeze and migrate to Astro/Starlight
    

The migration resulted in removing a net -19,624 lines of code from our maintenance burden.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3r9Hj2NwU40GPLTw5TbGYG/d292b405c097ebd577173f5d61c17d03/BLOG-2600_3.png)

While the number of files had grown substantially since our last major migration, our strategy was very similar to the 2021 migration. We used Markdown AST and astray, a utility to walk ASTs, created specifically for the previous migration!

## What we learned

A website migration like our move to Astro/Starlight is a complex process that requires time to plan, review, and coordinate, and our preparation paid off! Including our Cloudflare Community MVPs as part of the planning and review period proved incredibly helpful. They provided great guidance and feedback as we planned for the migration. We only needed one day of code freeze, and there were no rollbacks or major incidents. Visitors to the site never experienced downtime, and overall the migration was a major success.

During testing, we ran into several use cases that warranted using experimental Astro APIs. These APIs were always well documented, thanks to fantastic open source content from the Astro community. We were able to implement them quickly without impacting our release timeline.

We also ran into an edge case with build time performance due to the number of pages on our site (4000+). The Astro team was quick to triage the problem and begin investigation for a permanent fix. Their fast, helpful fixes made us truly grateful for the support from the Astro Discord server. A big thank you to the Astro/Starlight community!

## Contribute to developers.cloudflare.com!

Migrating developers.cloudflare.com to Astro/Starlight is just one example of the ways we prioritize world-class documentation and user experiences at Cloudflare. Our deep investment in documentation makes this a great place to work for technical writers, UX strategists, and many other content creators. Since adopting a content like a product strategy in 2021, we have evolved to better serve the open source community by focusing on inclusivity and transparency, which ultimately leads to happier Cloudflare users. 

We invite everyone to connect with us and explore these exciting new updates. Feel free to reach out if you’d like to speak with someone on the content team or share feedback about our documentation. You can share your thoughts or submit a pull request directly on the cloudflare-docs repository in GitHub.

Go to Source
