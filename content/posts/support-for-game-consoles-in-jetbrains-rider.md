---
title: "Support for Game Consoles in JetBrains Rider"
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

Big news for console game developers! As part of the 2024.3 release, JetBrains Rider added support for PlayStation®5 and Xbox consoles, allowing you to build, deploy, and debug your Unreal Engine and custom game engines directly on your preferred game consoles. Rider has excellent built-in support for working with C++, with lots of great functionality \[…\]

Big news for console game developers! As part of the 2024.3 release, JetBrains Rider added support for PlayStation®5 and Xbox consoles, allowing you to build, deploy, and debug your Unreal Engine and custom game engines directly on your preferred game consoles.

<iframe loading="lazy" title="JetBrains Rider for Consoles" width="500" height="281" src="https://www.youtube.com/embed/pNT1oqkS2Po?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Rider has excellent built-in support for working with C++, with lots of great functionality such as fast and accurate navigation, smart code completion, and hundreds of inspections and quick-fixes. If you’re using Unreal Engine, there’s even more, with specific inspections for reflection macros and RPC functions, Unreal Header Tool errors are shown as you type, and Rider can create Unreal classes without leaving the IDE. Rider will even index your blueprints to show where your classes are used and the values of serialised fields.

Now Rider has great support for working with Unreal Engine or custom game engines on PlayStation®5 and Xbox consoles, with Nintendo Switch support coming soon. These consoles are supported via separate plugins, which you can download once we’ve validated you with the appropriate developer programs. See the Rider for Consoles page for the forms.

Once installed, Rider can choose which device you want to work with.

![Rider's main toolbar showing a "connect to device" drop-down, including an Xbox](https://blog.jetbrains.com/wp-content/uploads/2025/03/rider-connect-to-console.png)

Everything is automatically set up, so clicking the _Debug_ button will build your project, deploy it, and launch it under the debugger. Click in the editor gutter to set a breakpoint, and when this is hit, Rider will show and modify the values of local variables, evaluate expressions, and let you step over and step into your code.

All of Rider’s existing debugger functionality, including watch variables, conditional breakpoints, and dependent breakpoints, works as expected. Make sure to use tracepoints as a quick way to log information without having to recompile and redeploy!

![Creating a conditional breakpoint in Rider by adding an equality condition for a property](https://blog.jetbrains.com/wp-content/uploads/2025/03/rider-conditional-breakpoint.png)

We’re very excited to introduce console support for Unreal Engine and native development, and we’re not the only ones!

> “At Warhorse Studios, working on Kingdom Come: Deliverance 2 means developing and debugging the game for not only PC but also for consoles. With Rider’s new debugger for Xbox and PlayStation®5, it is finally possible. It’s fantastic to have the flexibility of using a single IDE across all platforms we develop on. Rider has opened up a new level of convenience and efficiency for us.”
> 
> Petr Nohejl – Programmer at Warhorse Studios

Download Rider plugins for consoles

“PlayStation” is a registered trademark or trademark of Sony Interactive Entertainment Inc.

P.S. Don’t forget that Rider already supports debugging Unity games on consoles without additional plugins! Thanks to Unity’s IL2CPP support for the Mono debugging protocol, you can connect to your game consoles just like when debugging a Unity game on desktop or mobile devices.

Go to Source
