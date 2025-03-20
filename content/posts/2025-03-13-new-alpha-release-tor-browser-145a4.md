---
title: "New Alpha Release Tor Browser 145a4"
date: 2025-03-13T00:00:00Z
draft: false
type: posts
---
# New Alpha Release Tor Browser 145a4

<br/>

<br/>
 Tor Browser 14.5a4 is now available from the Tor Browser download page and also from our distribution directory. This version includes important security updates to
<br/>
  ![](https://blog.torproject.org/new-alpha-release-tor-browser-145a4/lead.png)

Tor Browser 14.5a4 is now available from the [Tor Browser download page](https://www.torproject.org/download/alpha/) and also from our [distribution directory](https://www.torproject.org/dist/torbrowser/14.5a4/).

This version includes important [security updates](https://www.mozilla.org/en-US/security/advisories/) to Firefox.

Send us your feedback
---------------------

If you find a bug or have a suggestion for how we could improve this release, [please let us know](https://support.torproject.org/misc/bug-or-feedback/).

Full changelog
--------------

The full changelog since [Tor Browser 14.5a3](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/raw/main/projects/browser/Bundle-Data/Docs-TBB/ChangeLog.txt) is:

-   All Platforms
    -   Updated OpenSSL to 3.0.16
    -   [Bug tor-browser#43446](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43446): Change the Tor Browser name between releases
    -   [Bug tor-browser#43463](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43463): Include moat circumvention countries in the build (tor-browser part)
    -   [Bug tor-browser#43487](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43487): Rebase Tor Browser Alpha onto 128.8.0esr
    -   [Bug tor-browser#43529](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43529): AutoBootstrapAttempt cancel does not await BootstrapAttempt.cancel
    -   [Bug tor-browser#43524](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43524): Enable new locales: be, bg and pt-PT
    -   [Bug tor-browser#43551](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43551): Backport Mozilla Bug 1924070 - modify H.264 extradata to match sample conversion code....
-   Windows + macOS + Linux
    -   Updated Firefox to 128.8.0esr
    -   [Bug tor-browser#40473](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/40473): Tor logs do not update in about:preferences#tor as new logs come in
    -   [Bug tor-browser#43205](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43205): newwin / letterboxing rounding with subpixels is off
    -   [Bug tor-browser#43328](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43328): Improve tor log dialog
    -   [Bug tor-browser#43465](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43465): Show the urlbar Connect button during a bootstrap or final error
    -   [Bug tor-browser#43469](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43469): Rename "Quickstart" toggle as "Connect automatically" (Desktop)
    -   [Bug tor-browser#43502](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43502): Move openTorConnect and getRedirectUrl to TorConnectParent
    -   [Bug tor-browser#43504](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43504): Implement User Survey UX (Desktop)
    -   [Bug tor-browser#43547](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43547): Cannot remove the final bridge
-   Linux
    -   [Bug tor-browser#30970](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/30970): Different window borders in XFCE can lead to different, not rounded window sizes
-   Android
    -   Updated GeckoView to 128.8.0esr
    -   Updated Zstandard to 1.5.7
    -   [Bug tor-browser#43329](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43329): Remove remaining traces of the old Bootstrap on Android
    -   [Bug tor-browser#43408](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43408): Access TorConnect.quickstart separately from TorSettings.getSettings on Android
    -   [Bug tor-browser#43480](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43480): Split up TorConnectionAssistViewModel for better readibility and performance.
    -   [Bug tor-browser#43498](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43498): Uplift tor-browser#43129: about:neterror cannot display SVG on Android with Security Level Safest
    -   [Bug tor-browser#43528](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43528): Improper handling of TorBootstrapChangeListener in HomeActivity
-   Build System
    -   All Platforms
        -   Updated Go to 1.23.7
        -   [Bug tor-browser-build#41040](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41040): Add configuration to rbm.conf to select channel and platforms
        -   [Bug tor-browser-build#41121](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41121): Use the official Go binaries for bootstrapping
        -   [Bug tor-browser-build#41372](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41372): Handle branding names in tor-browser-build
        -   [Bug tor-browser-build#41379](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41379): Include moat circumvention countries in the build (tor-browser-build part)
        -   [Bug tor-browser-build#41380](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41380): Update kick-devmole script to use Mullvad's new GitHub action
        -   [Bug tor-browser-build#41381](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41381): Usability improvements for the browser commit tagging script
        -   [Bug tor-browser-build#41382](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41382): Replace gitlab templates ReleasePrep label references with Apps::Type::ReleasePreparation
        -   [Bug tor-browser-build#41383](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41383): Add clairehurst to list of accepted firefox/geckoview signers
        -   [Bug tor-browser-build#41384](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41384): OpenSSL hash files have changed format
        -   [Bug tor-browser-build#41389](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41389): Remove need to update set-config.tbb-version
        -   [Bug rbm#40082](https://gitlab.torproject.org/tpo/applications/rbm/-/issues/40082): With `fetch: if_needed`, rbm is doing a git fetch when it shouldn't, when using a fixed commit
    -   Windows + macOS + Linux
        -   [Bug tor-browser-build#40799](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/40799): Remove legacy locale iteration in build and signing scripts
        -   [Bug tor-browser-build#41363](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41363): Change update-response generation script to create one commit per OS+arch tuple
        -   [Bug tor-browser-build#41374](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41374): Remove support for migrate\_archs and migrate\_langs in update\_responses
    -   Windows + Linux + Android
        -   [Bug tor-browser-build#41386](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41386): Upgrade Go to 1.23 for Windows, Linux, and Android
    -   Linux
        -   [Bug tor-browser-build#41337](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41337): Remove libstdc++ from Linux tor-expert-bundle
    -   Android
        -   [Bug tor-browser#42669](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/42669): Remove dependency on Application-Services
        -   [Bug tor-browser#43518](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/43518): Verify existence of localProperties.dependencySubstitutions.geckoviewTopsrcdir before substituting
        -   [Bug tor-browser-build#41387](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/issues/41387): Fix Golang toolchain breakage for lyrebird: linkname

-   [applications](https://blog.torproject.org/category/applications)
-   [releases](https://blog.torproject.org/category/releases)

#### [Source](https://blog.torproject.org/new-alpha-release-tor-browser-145a4/)

<br/>
---
