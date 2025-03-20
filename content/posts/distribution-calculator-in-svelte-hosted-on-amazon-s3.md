---
title: "Distribution Calculator In Svelte - Hosted on Amazon S3"
date: 2025-01-19
---

I recently completed building a Simple Calculator to help members calculate their distributions payable from OnePath. Every quarter or so, Onepath  
publishes a spreadsheet containing all the distributions payable for the managed funds which they sell. Customers have to go through the sheets, find their fund and copy the distribution cents per share, then do the calculation against the number of units they own.

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3dkp39mktefiq5jfcmph.png)

I think this process is quite tedious and outdated. It would be ideal if Onepath has an app, but building apps is not their primary business.

However they could expose their datasets via an API so that developers can build apps or solutions against their dataset. Building and maintaining APIs has it own issues so it also not in their core business to do this.

In order to build this solution I have had to design an API which is documented at Api

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzjwj37ig7fgbajv5rj91.png)

## Decision Register

Here are some decisions I made while building this solution.

| # | Decision | Reason |
| --- | --- | --- |
| 1 | Use Svelte over React | I decided to build this sample in svelte rather than react because for some reason, I have never wanted to build anything in react, I really dont know why, I never seemed to take to it, I have done a lot of tutorials and training in react, but for some reason I never could get myself to build anything in it. |
| 2 | Use Svelte over Vue | I had already built a solution in vue and vuex Visit Kate , Tim and Marty And Joel Podcast Library |
| 3 | Use SvelteKit | I started off with a single page application, as you normally do when using a todo app, however, I quickly realised that I would need multiple pages, so I had to recreate the solution using svelteKit. I would recommend that you always start with sveltekit. |
| 4 | Host on S3 | I have always found it easier to host single page application built on S3, Its quite a simple process to do. Furthermore a cloudfronrt distribution can be placed in front of the S3 bucket. Note that AWS has changed how permission is granted from cloudfront to S3 |
| 5 | Data Munging | I found it easy to convert the excel sheets to csv and then sql statements using simple php scripts |
| 6 | Building the backend Api | Backend Api was built using laravel. There is no longer the option to create api projects with Laravel Lumen so the Api was implemented in Laravel |
| 7 | Protecting the backend Api | The backend api was protected using Aws API Gateway |

I switched career from being a web developer to being an integration consultant. I didn't want to continue building SAAS because there became too many choices on how you can build and deploy them. Furthermore, the tool chains became too complicated. But I still like to build web applications in my spare time to keep up with the latest tech.  
While building this application, I made some use of Copilot to get though some svelte issues which had me stumped. Using copilot was a pleasant experience, I would recommend.

Go to Source
