---
title: "New Alpha Release Tor Browser 145a5"
date: 2025-03-21T00:00:00Z
draft: false
type: posts
---
# New Alpha Release Tor Browser 145a5





 Tor Browser 14.5a5 is now available from the Tor Browser download page and also from our distribution directory. This version includes the first version of

  ![](https://blog.torproject.org/new-alpha-release-tor-browser-145a5/lead.png)

Tor Browser 14.5a5 is now available from the [Tor Browser download page](https://www.torproject.org/download/alpha/) and also from our [distribution directory](https://www.torproject.org/dist/torbrowser/14.5a5/).

This version includes the first version of the connection assist for Android.

We invite you all to try out this new feature and we would really appreciate any feedback.

Send us your feedback
---------------------

If you find a bug or have a suggestion for how we could improve this release, [please let us know](https://support.torproject.org/misc/bug-or-feedback/).

Full changelog
--------------

The full changelog since [Tor Browser 14.5a4](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/raw/main/projects/browser/Bundle-Data/Docs-TBB/ChangeLog.txt) is:

-   All Platforms
    -   Updated Lyrebird to 0.6.0
    -   Updated Snowflake to 2.11.0
    -   [Bug tor-browser#42300](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/42300): Do not store logs inside TorProvider
    -   [Bug tor-browser#43488](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43488): Handle Moat connection errors and other non-bootstrapping errors in TorConnect
    -   [Bug tor-browser#43490](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43490): Use lower case "n" for "Tor network" in the UI
    -   [Bug tor-browser#43556](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43556): Update Desktop and Android survey dismissal string
    -   [Bug tor-browser#43575](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43575): Cleanup channel preferences
-   Windows + macOS + Linux
    -   [Bug tor-browser#41051](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/41051): Replace Noto Sans Myanmar with Pyidaungsu
    -   [Bug tor-browser#41755](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/41755): Show the link to about:support in the help menu
    -   [Bug tor-browser#42550](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/42550): Do not re-try auto-bootstrapping after the user selects a specific region in about:torconnect
    -   [Bug tor-browser#42656](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/42656): about:torconnect new url location override (maybeUpdateOpenLocationForTorConnect) mostly does nothing
    -   [Bug tor-browser#42670](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/42670): Letterboxing visible even if disable with tiled window managers
    -   [Bug tor-browser#42720](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/42720): Link to release notes missing from "About Tor Browser" window
    -   [Bug tor-browser#43321](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43321): Do not focus the connect button if the user has never connected before
    -   [Bug tor-browser#43405](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43405): Handle failing to apply tor settings
-   Android
    -   [Bug tor-browser#41188](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/41188): Implement Android-native Connection Assist UI
    -   [Bug tor-browser#42251](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/42251): Expose TorConnect lifecycle events to fenix
    -   [Bug tor-browser#43091](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43091): Delete unused android strings
    -   Bug 43361: \[Android\] Move code relating to `onTerminate()` in `FenixApplication.kt` \[tor-browser\]
    -   [Bug tor-browser#43473](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43473): Rename "Quickstart" toggle as "Connect automatically" (Android)
    -   [Bug tor-browser#43505](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43505): Implement User Survey UX (Android)
-   Build System
    -   All Platforms
        -   [Bug tor-browser-build#41394](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41394): Fix upload-update\_responses-to-staticiforme for mullvadbrowser
        -   [Bug tor-browser-build#41398](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41398): Build snowflake from main on nightlies
        -   [Bug tor-browser-build#41399](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41399): Update snowflake to 2.11.0 and lyrebird to 0.6.0
    -   Windows + macOS + Linux
        -   [Bug tor-browser-build#41401](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41401): Replace Noto Sans Myanmar with Pyidaungsu
    -   Android
        -   [Bug tor-browser-build#41402](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41402): Fix Snowflake 2.11.0 on Android

-   [applications](https://blog.torproject.org/category/applications)
-   [releases](https://blog.torproject.org/category/releases)

#### [Source](https://blog.torproject.org/new-alpha-release-tor-browser-145a5/)

