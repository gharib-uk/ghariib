---
title: "<div>Midori Browser 11.5 Added Headless Mode & New App Icon</div>"
date: 2025-01-08
tags: 
  - "browser"
  - "file"
  - "un"
  - "ux"
  - "windows"
---

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/midora-newicon-250x250.webp)

Midori web browser announced new 11.5 release recently with some exciting new features.

Midori was a popular lightweight web browser that was default in elementary OS and Bodhi Linux. It’s now a free open-source Firefox derived browser developed by Astian Foundation, and licensed under the Mozilla Public License (MPL).

The browser released new 11.5 recently, changed its app icon from a green lizard to **new flat design logo** that IMO feels better.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/midori-iconchange-700x270.webp)

Besides that, Midori 11.5 features headless mode. Meaning, **it can now work without a graphical user interface**, allowing developers and advanced users to use it for automated applications and development environments.

And, here’s an example usage for Python according to the screenshot in Astian website:

```
from selenium import webdriver
from selenium.webdriver.common.options import Options

# Configure options for Midori in headless mode.
options = Options()
options.add_argument('--headless')
options.add_argument('--disable-gpu')

# Initialize the Midori driver.
driver = webdriver.Chrome(options=options, executable_path='/route/a/midori-driver')

# Navigate to a web page
driver.get("https://www.example.com")

# Print the page title
print(driver.title)

# Close the browser
driver.quit()
```

The release also changed the color scheme for the address bar and focused tab. They are now highlighted with distinct-different colors, letting you know where you’re working with without taking too much attention.

The browser now **has a built-in ads blocker**, though disabled by default. User can enable it by going to _Privacy & Security_ settings under Enable DNS over HTTPS.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/midori-adsblocking-700x431.webp)

Other changes in the new browser release include:

- Update default AstianGO search engine with improved speed, accuracy of search results, as well as ability to search for maps.
- Integrated DNS browsing by default, maximizing privacy and security by blocking trackers.
- Performance boost between 20% and 30%.
- Hover opened tab to preview page with thumbnail.
- Improve colors in reading mode to help reading without straining the eyes
- Official OpenSUSE Build repository so far offers `.rpm` packages for openSUSE.

### How to Get Midori web browser:

The Github releases page offers universal AppImage package for Linux, `.deb` package for Debian, Ubuntu, Linux Mint, `.rpm` package for Fedora, openSUSE, RHEL, `pkg.tar.zst` package for Arch, as well as `.exe` file for Windows.

Download Midori (under ‘Assets’)

For choice, you may go to its website to download the installer packages, though they are still at last 11.4 at the moment of writing.

Go to Source
