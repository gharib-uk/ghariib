---
title: "PHP Annotated – January 2025"
date: 2025-02-01
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

Welcome to the January edition of PHP Annotated! This recap is carefully handcrafted and brings you the most interesting developments in the PHP community over the past couple of months, so you don’t have to sift through the noise, we’ve done it for you. Highlights PHP 8.4 PHP 8.4 was officially released on November 21, \[…\]

![PHP Annotated](https://blog.jetbrains.com/wp-content/uploads/2025/01/ps-featured_blog_1280x720_en.png)

Welcome to the January edition of PHP Annotated! This recap is carefully handcrafted and brings you the most interesting developments in the PHP community over the past couple of months, so you don’t have to sift through the noise, we’ve done it for you.

## Highlights

- ### PHP 8.4
    
    PHP 8.4 was officially released on November 21, 2024, and by now, version 8.4.3 is already available.
    
    This major language update brings many new features, such as property hooks, asymmetric visibility, an updated DOM API, performance improvements, bug fixes, and general code cleanup.
    
    If you want to learn more about all the goodies in the release, visit php.watch and stitcher.io.
    
    There are also some lesser known improvements that you can learn more about from the Tideways blog:
    
    - PHP 8.4 improves Closure Naming for simplified debugging.
    - What’s new in PHP 8.4 in terms of performance, debugging, and operations.
    
    **Install or upgrade to PHP 8.4**
    
    - Windows: Compiled binaries available at windows.php.net.
    - Fedora/RHEL/CentOS: Available as a software collection (php84) from the Remi repo.
    - macOS: PHP 8.4 can be installed via Homebrew using the shivammathur/homebrew-php tap.
    - Docker: PHP 8.4 images are now available on Docker Hub with 8.4 tags.
    - Herd also comes with PHP 8.4 supported.
    
    Watch a 📺 Celebrating PHP 8.4 stream with Nicolas, Brent, and Roman:  
    
    <iframe loading="lazy" width="560" height="315" src="https://www.youtube.com/embed/1AL2oDt9q38?si=0fCv3BcMeWpwGBMg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
    
- ### PHP 8.2 goes in security-fixes-only phase
    
    Starting this year, PHP versions now follow **a four-year support timeline**: two years of active support followed by two years of security fixes.
    
    For PHP 8.1 security patches will be provided until December 31, 2025, while PHP 8.2 will be maintained until December 31, 2026. The recent PHP 8.2.27 release marked the end of its active support phase.
    
- ### PHPStan 2.0 has been released
    
    This update introduces Level 10 code analysis with stricter handling of mixed types, and adds support for the List type. You can also expect reduced memory consumption and improved performance.
    
    Markus Staab shares interesting technical insights about improving PHPStan:  
    A mixed type PHPStan journey, PHPStan performance on different hardware, My new PHPStan focus: multi-phpversion support.
    
- ### 🎂 The PHP Foundation turned three
    
    The PHP Foundation was established three years ago. Over the past year, the PHP Foundation has supported the work of ten core developers, and made a significant contribution to the PHP language.
    
    Consider supporting the PHP Foundation via OpenCollective or GitHub Sponsors.
    
    The PHP Foundation has been also active with developments:
    
    - PHP 8.4: How Property Hooks Happened – Larry Garfield details the decade-long journey of implementing Property Hooks in PHP 8.4.
    - The PHP Installer for Extensions php/pie reached version 0.5!  
        Learn more about it from Pie: new extension installer for PHP post by Grzegorz Korba.
- ### Rector 2.0
    
    This major release updates dependencies (PHPStan 2 and PHP-Parser 5), runs 10–15% faster, and has 5 new features.
    
- ### FrankenPHP 1.3
    
    In this release, expect performance improvements, watcher mode, dedicated Prometheus metrics, and more.
    
- ### Conductor – Automatic dependency updates for Composer
    
    The Packagist team announced a new tool that is similar to Dependabot, but specifically tailored to PHP projects.
    
- ### You can now run code examples directly on the php.net website
    
    ![](https://blog.jetbrains.com/wp-content/uploads/2025/01/Screenshot-2025-01-28-at-10.19.06-AM.png)
    

## PHP Core

- ### ✅ RFC: Add persistent curl share handles
    
    PHP 8.5 will bring a new function `curl_share_init_persistent()`, which would allow cURL handles to be stored in global memory and reused in subsequent requests.
    
    Persistence allows PHP scripts to eliminate the overhead of establishing a connection (and DNS lookups, SSL session IDs, etc.) and can improve performance and reliability.
    
- ### ✅ RFC: Support Closures in constant expressions
    
    In PHP 8.5 it will be possible to use closures in previously
    
    - In attribute parameters,
    - As default values of properties and parameters.
    - Constants and class constants.
    
    ```
    <?php
    
    class Foo
    {
        public static Closure $callback = static function ($item) { echo "Hello world"; };
    }
    
    
    function my_array_filter(
        array $array,
        Closure $callback = static function ($item) { return !empty($item); },
    ) {
        // ...
    }
    
    ```
    
- ### ✅ RFC: Attributes on Constants
    
    Attributes were first introduced in the RFC: Attributes v2. Daniel Scherzer proposes to add support for attributes on compile-time non-class constants.
    
    ```
    #[MyAttribute]
    const EXAMPLE = 1;
    
    ```
    
- ### ✅ RFC: Error Backtraces v2
    
    Previously, unlike exceptions, PHP errors did not provide backtraces, which made it difficult to figure out their underlying cause. PHP 8.5 will come with a new ini setting `fatal_error_backtraces=1` which will generate detailed error messages and trace for `E_ERROR`s.
    
- ### 📣 RFC: Records vs. RFC: Data Class vs. RFC: Structs
    
    Apparently, there is a big interest in the community to add a simple native way of creating Value Objects in PHP.
    
    Which one would you prefer?
    
- <iframe loading="lazy" width="560" height="315" src="https://www.youtube.com/embed/BvAcP6RtlAA?si=czt_aNe3rM29YuMr" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
    

## Tools

- ringPHP/php-mrloop – A PHP port of the mrloop eventware designed to harness the powers of `io_uring`. Read an Introducing ext-mrloop blog post to learn why io\_ring is better than `select()`, `poll()`, and `epoll()` async io implementations.
- loupe-php/loupe – A full text search engine with tokenization, stemming, typo tolerance, filters, and geo support based on only PHP and SQLite.
- twigstan/twigstan – TwigStan is a static analyzer for Twig templates powered by PHPStan.
- azjezz/psl – A modern, consistent, centralized, well-typed, non-blocking set of APIs for PHP programmers.
- Full LAMP Sandbox – (PHP + WebServer + DB) running 100% inside your browser.
- carthage-software/mago – A toolchain for PHP that aims to provide a set of tools to help developers write better code. Built in Rust.
- smoqadam/pvm – A simple bash script to manage multiple PHP versions on Linux and macOS.
- tnylea/php-ext – A Chrome extension to show PHP (Laravel) devtools console.
- phikiphp/phiki – Syntax highlighting powered by TextMate grammars in PHP.
- coduo/php-humanizer – A useful tool to transform different strings and numbers to human-readable form.
- RoadRunner v2024.3.0 – The PHP application server got a major update with auto workers scaling.

## AI

- echolabsdev/prism – A unified interface for working with LLMs in Laravel. Supports Anthropic, DeepSeek, Gemini, Groq, Mistral, Ollama, OpenAI, and xAI APIs.
    
    Also extensible with jordan-price/toolbox.
    
- CodeWithKyrian/whisper.php – Local Speech to Text in PHP made easy thanks to Whisper.cpp and OpenAI.
- deepseek-php/deepseek-php-client – Supercharged community-maintained PHP API client that allows you to interact with deepseek API.

## PhpStorm

- ### PhpStorm 2024.3 is now available
    
    The new PhpStorm comes with full PHP 8.4 support, inline AI prompts, Laravel Herd support.
    
    Support for `.env` files is now built into PhpStorm. Previously it required installing a separate plugin.
    
    JetBrains also announced a closed beta for **Junie, AI Coding Agent** for IDEs.
    
- MetaStorm – This plugin allows extending PhpStorm’s behaviour and adding support for your custom frameworks with a few lines in a config file. It unlocks both references and autocompletion at regular places such as `method($object,), render()`, etc.
- buggregator/phpstorm-plugin – This plugin works in the pair with buggregator/trap and allows dumping and debugging PHP projects just inside the IDE. Supports VarDumper server, Xhprof profiler, local SMTP server, local Sentry, and much more.
- Cron & Crontab Support Plugin

## Frameworks

- ### Symfony 7.2.0 has been released
    
    Check the Living on the Edge category on this blog to learn about the main features of this new stable release.
    
- ### Drupal CMS
    
    Previously known as Drupal Starshot Initiative, Drupal CMS is the new way of creating web apps based on Drupal with no-code building experience.
    
- thedevdojo/wave – The SaaS starter kit based on Laravel.
- WordPress as a git repo by Adam Zieliński – A promising addition that might be landed in WordPress core, allowing using markdown files as a backend for WordPress site.
- How Geocodio keeps 300M addresses up to date with Laravel and SQLite.
- Naoray/laravel-github-monolog – Laravel log Channel for GitHub issues.

## Misc

- Building Maintainable PHP Applications: Data Transfer Objects by Davor Minchorov.
- Playtime with PHP Attributes by Pete Wond.
- Why Final Classes make Rector and PHPStan more powerful by Tomas Votruba.
- Property Hooks in PHP 8.4: Game Changer or Hidden Trap? by David Grudl.
- PHP version stats: January, 2025 by Brent.
- Unleash: Feature flags in PHP by Dominik Chrástecký.
- The Dangers of PHP’s unserialize and How to stay safe by Sheikh Heera.
- Stop using Pseudo-Types by Frédéric Bouchery.
- Importing 1.7 billion rows of CSV data from Stripe by Jon Purvis.
- azjezz/php-pretty-diff – A nice demo repository on how to use Rust code in a PHP project with FFI.
- TIL: You can use #️⃣ emoji instead of `#` symbol in comments and attributes in PHP!  
    ![](https://blog.jetbrains.com/wp-content/uploads/2025/01/Screenshot-2025-01-29-at-12.09.52-PM.png)

## Conferences

These PHP events are all worth a visit, and some are still accepting presentation proposals:

- Laracon EU 2025 – Amsterdam, The Netherlands, February 3–4.
- PHP UK Conference 2025 – London, UK, February 19.
- Laracon India 2025 – Ahmedabad, India, March 8–9. CFP
- PHP Conference Odawara 2025 – Japan, April 12.
- php\[tek\] 2025 – Chicago, IL, USA, May 20–22.
- PHPers Summit 2025 – Poznań, Poland, May 24–25. CFP
- Summer Camp – Opatija, Croatia, July 3–5. CFP 🆕

To find a PHP meetup happening near you, check out the calendar on php.net.

## Fun

- <iframe loading="lazy" width="560" height="315" src="https://www.youtube.com/embed/AJRGxd9cVaY?si=60IjCDT3uftQYIRo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
    
- <iframe loading="lazy" width="560" height="315" src="https://www.youtube.com/embed/Jk8q7MNeWeQ?si=9iSLt_LsVgPz9chx" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
    

* * *

If you have any interesting or useful links to share via PHP Annotated, please leave a comment on this post or let us know on X/Twitter.

Subscribe to PHP Annotated

![](https://blog.jetbrains.com/wp-content/uploads/2022/07/php-annotated-roman.png)

#### Roman Pronskiy

Developer Advocate at @PhpStorm, Executive Director at @The PHP Foundation.

Twitter | GitHub

Go to Source
