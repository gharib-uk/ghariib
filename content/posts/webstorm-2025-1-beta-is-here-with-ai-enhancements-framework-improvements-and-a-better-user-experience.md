---
title: "WebStorm 2025.1 Beta Is Here With AI Enhancements, Framework Improvements, and a Better User Experience"
date: 2025-03-19
categories: 
  - "development"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "linux"
  - "software"
---

The EAP cycle for WebStorm 2025.1 is over, and we’re excited to share what’s included in the Beta builds. The Toolbox App is the easiest way to get the Beta builds and keep both them and your stable versions up to date. You can also manually download the Beta builds from our website. DOWNLOAD WEBSTORM \[…\]

The EAP cycle for WebStorm 2025.1 is over, and we’re excited to share what’s included in the Beta builds.

The Toolbox App is the easiest way to get the Beta builds and keep both them and your stable versions up to date. You can also manually download the Beta builds from our website.

DOWNLOAD WEBSTORM 2025.1 BETA

**Important! WebStorm Beta builds are not fully tested and might be unstable.** Please try the latest build and share your feedback with us. You can do so using our issue tracker or by leaving a comment on this blog post.

## Key improvements

### AI Assistant

#### Сlaude and local LLMs

JetBrains AI Assistant is advancing its line of models! We’ve added support for Claude 3.5 Sonnet and Claude 3.5 Haiku, now provisioned in Amazon Bedrock. This means you’ll benefit from sharper responses, faster insights, and an even smoother experience. AI Assistant’s lineup of OpenAI models now includes o1, o1-mini, and o3-mini.

> **Update**: A new version of the JetBrains AI Assistant plugin has been released, bringing support for even more models, including:  
> • Claude 3.7 Sonnet  
> • OpenAI GPT-4.5  
> • Gemini 2.0 Flash

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-ai-models.png)

In addition to cloud-based models, you can now connect the AI chat to local models available through Ollama and LM Studio! You can set up local providers via _Settings_ | _Tools_ | _AI Assistant_ | _Custom Models:_

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-local-llms.png)

#### Improved AI completion for web frameworks

For the 2025.1 release we have focused on improving AI-based completion in the context of web framework components. These changes affect local full line code completion as well as cloud-based completion suggestions:

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-ai-completion-1.png)

#### Enhanced AI test generation

AI-powered test generation now offers more careful framework detection, especially for cases when multiple frameworks are present. Additionally, generated tests respect naming conventions:

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-AI-test-gen.gif)

### TypeScript

#### Service-powered type engine

The engine previously known as WebStorm@next or the _Use types from server_ option now has an official name – the service-powered type engine! We’re currently polishing it, with a focus on performance. A dedicated icon in the status bar displays the engine’s status, and you can granularly enable or disable it for supported frameworks or for TypeScript in general within the current project:

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-TypeEngine.png)

If you notice any degradations when the new engine is enabled, please use the _Submit a bug report_ link.

### Angular improvements

WebStorm now supports code completion for host binding attributes based on directive selectors. Quick-fixes for creating fields are also available within binding expressions. Moreover, refactoring is supported across different locations and is even available for CSS classes:

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-Angular-host-binding.gif)

The long-awaited support for Reactive Forms is here. This update includes code completion, syntax highlighting, validation, refactoring, and quick-fixes for Reactive Forms. Both declaration styles – constructor-based and builder-based – are fully supported.

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-reactive-forms.gif)

There is also a new intention to extract or inline component templates. Invoke _Show Context Actions_ (_⌥⏎_ (macOS) / _Alt+Enter_ (Windows/Linux)) to use the action:

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-Angular-templates.gif)

This release brings many other improvements and bug fixes for Angular beyond the features mentioned above.

### Next.js improvements

WebStorm 2025.1 Beta introduces automatic run configuration creation for Next.js applications. Now, you can easily initiate debug sessions for both the client and server components of your Next.js application using the _Run widget_.

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-NextJS-debug-1.png)

### Vue improvements

The _New Project_ wizard now gives you the option to generate a new Nuxt project using the Nuxt CLI (`nuxi`):

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-Nuxt.png)

We’ve also implemented several bug fixes, including:

- WEB-59818 – Vue custom global properties added by augmenting `vue` are now resolved.

- WEB-69114 – Auto-completion and auto-imports now work for packaged components declared with `__VLS_WithTemplateSlots`.

### User experience

#### Floating toolbar

In WebStorm 2025.1 Beta, invoking _Show Context Actions_ (⌥⏎ (macOS) / Alt+Enter (Windows/Linux)) now opens the floating toolbar with different action groups. The toolbar also appears when you select code in the editor:

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-floaing-toolbar.png)

The floating toolbar contains the following actions and action groups:

- Context actions

- AI Assistant context actions (if the AI Assistant plugin is installed)

- Refactor

- Show usages

- Surround with tag

- Reformat code

You can customize the contents of the toolbar by opening the kebab menu (three vertical dots) and selecting the _Customize Toolbar…_ option

#### New file creation in the Project tool window

Creating new files is now easier in the _Project_ tool window. You can simply use the + icon located directly in the window’s toolbar:

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-new-file-from-project-view.png)

### GraphQL and Prisma

WebStorm 2025.1 Beta introduces the following improvements for Prisma:

- ULID support (WEB-71197)

- Multi-line comment syntax (WEB-70929)

- `gql(query)` syntax support (WEB-71611)

### Prettier integration improvements

WebStorm now automatically detects and applies the nearest in the directory tree Prettier configuration when formatting files, ensuring the code style is consistent across subprojects. The Indentation status bar widget shows whether Prettier has modified the code style. The new dropdown menu offers the following options:

- _Open Configuration File_ (or use default Prettier settings if none exists)

- _Open Prettier Settings_

- _Disable Prettier Code Style Modifications_

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-prettoer-widget.png)

Code style changes made by Prettier are visible in the _Code Style_ tab under _Settings_, where you can also disable automatic modifications.

![](https://blog.jetbrains.com/wp-content/uploads/2025/03/WS251-Prettier-settings.png)

A new setting controls whether WebStorm should use Prettier to auto-format code styles. This option is enabled by default. In case if `.editorconfig` is present, Prettier takes precedence for files it handles, but if certain options aren’t specified in Prettier, EditorConfig settings still apply.

### Monorepo support

We are consistently working to improve reliability and performance in mono-repo setups. Here are some of the most noticable fixes in this build for long-standing issues:

- WEB-71210 – Auto-import and syntax highlighting now work as expected for sibling packages in monorepo.

- WEB-70868 – IntelliSense fixed in large nx TypeScript monorepos.

- WEB-69642 – Array values in `package.json` export fields are now processed properly.

- WEB-64647 – Path aliases defined in `package.json` exports are now used by auto-import in monorepos.

That’s it for today. For the full list of improvements introduced throughout the latest 2025.1 EAPs, check out the release notes.

_The WebStorm team_

Go to Source
