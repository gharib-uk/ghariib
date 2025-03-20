---
title: "Bringing multiple windows to Flutter desktop apps"
date: 2025-01-22
---

Over the past 5 years, Canonical has been contributing to Flutter, including building out Linux support for Flutter applications, publishing libraries to help integrate into the Linux desktop and building modern applications for Ubuntu, including our software store. Last year we announced at the Ubuntu Summit that we’ve been working on bringing support for multiple windows to Flutter desktop apps.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1568,h_1600/https://ubuntu.com/wp-content/uploads/94a6/image.png)

## Why multiple windows support for Flutter desktop apps is needed

One current limitation that Flutter desktop apps have is that they are confined to a single window. This makes sense on mobile where an app takes up the whole screen but for Flutter desktop apps there’s much more space to take advantage of. We know that many members of the Flutter community – including us here at Canonical – have been waiting patiently to break out of that single window.

Canonical has a long history of working with graphical environments having produced Ubuntu Desktop for over 20 years. We want to make sure that the Flutter multi-window support works across a diverse range of desktops including all of those across the extremely varied Linux ecosystem. We’re also thinking ahead about how to make sure Flutter desktop apps continue to work well as the concept of a desktop becomes more diverse.

## Proposed solution for Flutter desktop apps

Desktop applications are made up of multiple windows that are used for all sorts of things, including tooltips, dialogs and menus. In the comparison below you can see the same Flutter desktop app running with the current version of Flutter on the left, and the multi-window version on the right. Notice how the app on the right feels more integrated: menus and tooltips are better aligned to the mouse cursor instead of being shifted or cropped to fit inside the window.

<figure>

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1600,h_900/https://ubuntu.com/wp-content/uploads/5f1a/image-1.png)

<figcaption>

Comparison of single window (old) vs multi-window (new)

</figcaption>

</figure>

The best thing about the approach we’ve taken is both apps above are using the same standard Flutter Material widgets – the multi-window support is applied automatically. If the app is run in a situation where multi-window is not applicable (e.g. mobile), the app will revert to the traditional behaviour.

When you’re ready to build a more complicated multi-window app this is easy to do as each window just fits into the Flutter widget tree.

Details for seasoned Flutter developers: you’ll need to make a small update to the runner, and if you have an unmodified runner this is easy to migrate. A small change is also required to the main function in the Dart code.

## Rolling this out

We’re currently reviewing the changes to the Flutter engine and framework. We’re working on a way to easily test these changes but if you’re up for the challenge go ahead and build our branches and test it yourself. 

When these changes have landed, building using the latest version of Flutter will enable your app to use multi-window on the Windows operating system. We are hard at work expanding this availability, and we will soon release multi-window support to both Linux and MacOS-based platforms.

We look forward to seeing what amazing apps you will build in the future!

Go to Source
