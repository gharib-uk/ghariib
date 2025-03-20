---
title: "ApostropheCMS 4.14.0: Smarter Content Management, Better Performance, and CSV Table Imports"
date: 2025-03-19
---

Hello Dev.to community!

ApostropheCMS 4.14.0 has been released. Check out an overview.

<iframe width="710" height="399" src="https://www.youtube.com/embed/t59TFCNtTDw"></iframe>

There is also a full deep dive of the release on our blog

From enhanced table support in the rich-text editor to better piece management, this release brings several quality-of-life improvements for both developers and content managers.

## Rich Text Improvements

The heart of content creation in Apostrophe has always been our rich text editor, and with this release, it becomes even more powerful. We've added the ability to generate tables from imported CSV files directly within the rich text widget. This allows content editors to quickly transform spreadsheet data into well-formatted web content.

We've also reworked rich text popovers to use `AposContextMenu` for both toolbar components and insert menu items, fixing problems seen when editing on smaller screen sizes.

## A More Efficient and Intuitive UI

This release also brings several usability improvements. One of the more noticeable changes is the increase in the default pagination size for piece-type managers, jumping from 10 to 50 items per page. This change significantly reduces the need for excessive clicking and increases efficiency when managing extensive collections of content.

Finding the right PDF is now faster with the addition of a "Tags" filter to the file manager. We've also improved the image manager’s infinite scroll pagination for a more reliable browsing experience.

## Quality-of-Life Fixes and Enhancements

No update is complete without some essential fixes. The `lang` attribute of the page `<html>` tag now correctly reflects the active localization settings, ensuring better accessibility and SEO compliance. We've also fine-tuned focus styling within the `AposTable` headers and added better error handling for misconfigured widgets in expanded preview mode. Another small but impactful fix: altering CSS styling to prevent margin collapse in nested areas, especially when the nested area comes from another document. This would sometimes make adding a peer widget above or below a parent widget difficult or impossible.

The `postcss-viewport-to-container-toggle` plugin was improved with expanded handling and conversion of more media queries and units, making our breakpoint preview feature more robust and versatile across a broader range of design scenarios.

🚀 Happy coding!

- Our blog
- GitHub Discussion
- YouTube Video

Go to Source
