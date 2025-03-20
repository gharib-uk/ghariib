---
title: "<div>The Co-Create Program: How customers are collaborating to build GitLab</div>"
date: 2025-02-01
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

This past year, over 800 community members have made more than 3,000 contributions to GitLab. These contributors include team members from global organizations like Thales and Scania, who are helping shape GitLab's future through the Co-Create Program — GitLab's collaborative development program where customers work directly with GitLab engineers to contribute meaningful features to the platform.

Through workshops, pair programming sessions, and ongoing support, program participants get hands-on experience with GitLab's architecture and codebase while solving issues or improving existing features.

"Our experience with the Co-Create Program has been incredible," explains Sébastien Lejeune, open source advocate at Thales. "It only took two months between discussing our contribution with a GitLab Contributor Success Engineer and getting it live in the GitLab release."

In this post, we'll explore how customers have leveraged the Co-Create Program to turn their ideas into code, learning and contributing along the way.

## The Co-Create experience

The GitLab Development Kit (GDK) helps contributors get started developing on GitLab. "The advice I would give new contributors is to remember that you can't break anything with the GDK," says Hook. "If you make a change and it doesn't work, you can undo it or start again. The beauty of GDK is that you can tinker, test, and learn without worrying about the environment."

Each participating organization in the Co-Create Program receives support throughout their contribution journey:

- **Technical onboarding workshop**: A dedicated session to set up the GitLab Development Kit (GDK) and understand GitLab's architecture
- **1:1 engineering support**: Access to GitLab engineers for pair programming and technical guidance
- **Architecture deep dives**: Focused sessions on specific GitLab components relevant to the issue the organization is contributing to
- **Code review support**: Detailed feedback and guidance through the merge request process
- **Regular check-ins**: Ongoing collaboration to ensure progress and address any challenges

This structure ensures that teams can contribute effectively, regardless of their prior experience with GitLab's codebase or the Ruby/Go programming language. As John Parent from Kitware notes, "If you've never seen or worked with GitLab before, you're staring at a sophisticated architecture and so much code across different projects. The Co-Create Program helps distill what would take weeks of internal training into a targeted crash course."

The result is a program that not only helps deliver new features but also builds lasting relationships between GitLab and its user community. "It's inspiring for our engineers to see the passion our customers bring to contributing to and building GitLab together," shares Shekhar Patnaik, principal engineer at GitLab. "Customers get to see the 'GitLab way,' and engineers get to witness their commitment to shaping the future of GitLab."

## Enhancing project UX with Thales

When Thales identified opportunities to improve GitLab's empty project UI, they didn't just file a feature request — they built the solution themselves. Their contributions focused on streamlining the new project setup experience by simplifying SSH/HTTPS configuration with a tabbed interface and adding copy/paste functionality for the code snippets. These changes had a significant impact on developer workflows.

The team's impact extended beyond the UX improvements. Quentin Michaud, PhD fellow for cloud applications on the edge at Thales, contributed to improving the GitLab Development Kit (GDK). As a package maintainer for Arch Linux, Michaud's expertise helped improve GDK's documentation and support its containerization efforts, making it easier for future contributors to get started.

"My open source experience helped me troubleshoot GDK's support for Linux distros,” says Michaud. “While improving package versioning documentation, I saw that GitLab's Contributor Success team was also working to set up GDK into a container. Seeing our efforts converge was a great moment for me — it showed how open source collaboration can help build better solutions."

The positive experience for the Thales team means that Lejeune now uses the Co-Create Program as "a powerful example to show our managers the return on investment from open source contributions."

## Advancing package support with Scania

When Scania needed advanced package support in GitLab, they saw an opportunity to contribute and build it themselves.

"As long-time GitLab users who actively promote open source within our organization, the Co-Create Program gave us a meaningful way to contribute directly to open source," shares Puttaraju Venugopal Hassan, solution architect at Scania.

The team started with smaller changes to familiarize themselves with the codebase and review process, then progressed to larger features. "One of the most rewarding aspects of the Co-Create Program has been looking back at the full, end-to-end process and seeing how far we've come," reflects Océane Legrand, software developer at Scania. "We started with discovery and smaller changes, but we took on larger tasks over time. It's great to see that progression."

Their contributions include bug fixes for the package registry and efforts to enhance the Conan package registry feature set, bringing it closer to general availability (GA) readiness while implementing Conan version 2 support. Their work and collaboration with GitLab demonstrates how the Co-Create Program can drive significant improvements to GitLab’s package registry capabilities.

"From the start, our experience with the Co-Create Program was very organized. We had training sessions that guided us through everything we needed to contribute. One-on-one sessions with a GitLab engineer also gave us an in-depth look at GitLab’s package architecture, which made the contribution process much smoother," said Juan Pablo Gonzalez, software developer at Scania.

The impact of the program goes beyond code — program participants are also building valuable skills as a direct result of their contributions. In the GitLab 17.8 release, both Legrand and Gonzalez were recognized as GitLab MVPs. Legrand talked about how the work she's doing in open source impacts both GitLab and Scania, including building new skills for her and her team: "Contributing through the Co-Create Program has given me new skills, like experience with Ruby and background migrations. When my team at Scania faced an issue during an upgrade, I was able to help troubleshoot because I'd already encountered it through the Co-Create Program."

## Optimizing authentication for high-performance computing with Kitware

Kitware brought specialized expertise from their work with national laboratories to improve GitLab's authentication framework. Their contributions included adding support for the OAuth2 device authorization grant flow in GitLab, as well as implementing new database tables, controllers, views, and documentation. This contribution enhances GitLab's authentication options, making it more versatile for devices without browsers or with limited input capabilities.

"The Co-Create Program is the most efficient and effective way to contribute to GitLab as an external contributor," shares John Parent, R&D engineer at Kitware. "Through developer pairing sessions, we found better implementations that we might have missed working alone."

As a long-time open source contributor, Kitware particularly appreciated GitLab's approach to development. "I assumed GitLab wouldn't rely on out-of-the-box solutions at its scale, but seeing them incorporate a Ruby dependency instead of building a custom in-house solution was great,” says Parent. “Coming from the C++ world, where package managers are rare, it was refreshing to see this approach and how straightforward it could be."

## Building better together: Benefits of Co-Create

The Co-Create Program creates value that flows both ways. "The program bridges a gap between us as GitLab engineers and our customers," explains Imre Farkas, staff backend engineer at GitLab. "As we work with them, we hear their day-to-day challenges, the parts of GitLab they rely on, and where improvements can be made. It's great to see how enthusiastic they are about getting involved in building GitLab with us."

This collaborative approach also accelerates GitLab's development. As Shekhar Patnaik, principal engineer at GitLab, observes: "Through Co-Create, our customers are helping us move our roadmap forward. Their contributions allow us to deliver critical features faster, benefitting our entire user base. As the program scales, there's a real potential to accelerate development on our most impactful features by working alongside the very people who rely on them."

## Get started with Co-Create

Ready to turn your feature requests into reality? Whether you're looking to enhance GitLab's UI like Thales, improve package support like Scania, or optimize authentication like Kitware, the Co-Create Program welcomes organizations who want to actively shape GitLab's future while building valuable open source experience.

Contact your GitLab representative to learn more about participating in the Co-Create Program, or visit our Co-Create page for more information.

This past year, over 800 community members have made more than 3,000 contributions to GitLab. These contributors include team members from global organizations like Thales and Scania, who are helping shape GitLab's future through the Co-Create Program — GitLab's collaborative development program where customers work directly with GitLab engineers to contribute meaningful features to the platform.

Through workshops, pair programming sessions, and ongoing support, program participants get hands-on experience with GitLab's architecture and codebase while solving issues or improving existing features.

"Our experience with the Co-Create Program has been incredible," explains Sébastien Lejeune, open source advocate at Thales. "It only took two months between discussing our contribution with a GitLab Contributor Success Engineer and getting it live in the GitLab release."

In this post, we'll explore how customers have leveraged the Co-Create Program to turn their ideas into code, learning and contributing along the way.

## The Co-Create experience

The GitLab Development Kit (GDK) helps contributors get started developing on GitLab. "The advice I would give new contributors is to remember that you can't break anything with the GDK," says Hook. "If you make a change and it doesn't work, you can undo it or start again. The beauty of GDK is that you can tinker, test, and learn without worrying about the environment."

Each participating organization in the Co-Create Program receives support throughout their contribution journey:

- **Technical onboarding workshop**: A dedicated session to set up the GitLab Development Kit (GDK) and understand GitLab's architecture
- **1:1 engineering support**: Access to GitLab engineers for pair programming and technical guidance
- **Architecture deep dives**: Focused sessions on specific GitLab components relevant to the issue the organization is contributing to
- **Code review support**: Detailed feedback and guidance through the merge request process
- **Regular check-ins**: Ongoing collaboration to ensure progress and address any challenges

This structure ensures that teams can contribute effectively, regardless of their prior experience with GitLab's codebase or the Ruby/Go programming language. As John Parent from Kitware notes, "If you've never seen or worked with GitLab before, you're staring at a sophisticated architecture and so much code across different projects. The Co-Create Program helps distill what would take weeks of internal training into a targeted crash course."

The result is a program that not only helps deliver new features but also builds lasting relationships between GitLab and its user community. "It's inspiring for our engineers to see the passion our customers bring to contributing to and building GitLab together," shares Shekhar Patnaik, principal engineer at GitLab. "Customers get to see the 'GitLab way,' and engineers get to witness their commitment to shaping the future of GitLab."

## Enhancing project UX with Thales

When Thales identified opportunities to improve GitLab's empty project UI, they didn't just file a feature request — they built the solution themselves. Their contributions focused on streamlining the new project setup experience by simplifying SSH/HTTPS configuration with a tabbed interface and adding copy/paste functionality for the code snippets. These changes had a significant impact on developer workflows.

The team's impact extended beyond the UX improvements. Quentin Michaud, PhD fellow for cloud applications on the edge at Thales, contributed to improving the GitLab Development Kit (GDK). As a package maintainer for Arch Linux, Michaud's expertise helped improve GDK's documentation and support its containerization efforts, making it easier for future contributors to get started.

"My open source experience helped me troubleshoot GDK's support for Linux distros,” says Michaud. “While improving package versioning documentation, I saw that GitLab's Contributor Success team was also working to set up GDK into a container. Seeing our efforts converge was a great moment for me — it showed how open source collaboration can help build better solutions."

The positive experience for the Thales team means that Lejeune now uses the Co-Create Program as "a powerful example to show our managers the return on investment from open source contributions."

## Advancing package support with Scania

When Scania needed advanced package support in GitLab, they saw an opportunity to contribute and build it themselves.

"As long-time GitLab users who actively promote open source within our organization, the Co-Create Program gave us a meaningful way to contribute directly to open source," shares Puttaraju Venugopal Hassan, solution architect at Scania.

The team started with smaller changes to familiarize themselves with the codebase and review process, then progressed to larger features. "One of the most rewarding aspects of the Co-Create Program has been looking back at the full, end-to-end process and seeing how far we've come," reflects Océane Legrand, software developer at Scania. "We started with discovery and smaller changes, but we took on larger tasks over time. It's great to see that progression."

Their contributions include bug fixes for the package registry and efforts to enhance the Conan package registry feature set, bringing it closer to general availability (GA) readiness while implementing Conan version 2 support. Their work and collaboration with GitLab demonstrates how the Co-Create Program can drive significant improvements to GitLab’s package registry capabilities.

"From the start, our experience with the Co-Create Program was very organized. We had training sessions that guided us through everything we needed to contribute. One-on-one sessions with a GitLab engineer also gave us an in-depth look at GitLab’s package architecture, which made the contribution process much smoother," said Juan Pablo Gonzalez, software developer at Scania.

The impact of the program goes beyond code — program participants are also building valuable skills as a direct result of their contributions. In the GitLab 17.8 release, both Legrand and Gonzalez were recognized as GitLab MVPs. Legrand talked about how the work she's doing in open source impacts both GitLab and Scania, including building new skills for her and her team: "Contributing through the Co-Create Program has given me new skills, like experience with Ruby and background migrations. When my team at Scania faced an issue during an upgrade, I was able to help troubleshoot because I'd already encountered it through the Co-Create Program."

## Optimizing authentication for high-performance computing with Kitware

Kitware brought specialized expertise from their work with national laboratories to improve GitLab's authentication framework. Their contributions included adding support for the OAuth2 device authorization grant flow in GitLab, as well as implementing new database tables, controllers, views, and documentation. This contribution enhances GitLab's authentication options, making it more versatile for devices without browsers or with limited input capabilities.

"The Co-Create Program is the most efficient and effective way to contribute to GitLab as an external contributor," shares John Parent, R&D engineer at Kitware. "Through developer pairing sessions, we found better implementations that we might have missed working alone."

As a long-time open source contributor, Kitware particularly appreciated GitLab's approach to development. "I assumed GitLab wouldn't rely on out-of-the-box solutions at its scale, but seeing them incorporate a Ruby dependency instead of building a custom in-house solution was great,” says Parent. “Coming from the C++ world, where package managers are rare, it was refreshing to see this approach and how straightforward it could be."

## Building better together: Benefits of Co-Create

The Co-Create Program creates value that flows both ways. "The program bridges a gap between us as GitLab engineers and our customers," explains Imre Farkas, staff backend engineer at GitLab. "As we work with them, we hear their day-to-day challenges, the parts of GitLab they rely on, and where improvements can be made. It's great to see how enthusiastic they are about getting involved in building GitLab with us."

This collaborative approach also accelerates GitLab's development. As Shekhar Patnaik, principal engineer at GitLab, observes: "Through Co-Create, our customers are helping us move our roadmap forward. Their contributions allow us to deliver critical features faster, benefitting our entire user base. As the program scales, there's a real potential to accelerate development on our most impactful features by working alongside the very people who rely on them."

## Get started with Co-Create

Ready to turn your feature requests into reality? Whether you're looking to enhance GitLab's UI like Thales, improve package support like Scania, or optimize authentication like Kitware, the Co-Create Program welcomes organizations who want to actively shape GitLab's future while building valuable open source experience.

Contact your GitLab representative to learn more about participating in the Co-Create Program, or visit our Co-Create page for more information.

Go to Source
