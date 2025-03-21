---
title: "Pip install and Pythons externally managed"
date: Thu, 06 Jul 2023 00:00:00 +0000
draft: false
type: posts
---
# Pip install and Pythons externally managed
https://www.kali.org/blog/python-externally-managed/images/banner-python-externally-managed.jpg
<br/>

<br/>
TL;DR: pip install is on the way out. Installing Python packages must be done via APT, aka. Kali Linux&rsquo;s package manager. Python packages coming from other sources should be installed in virtual environments. Long story below. Some background Back in February this year, for a few days, some of you might have tried
<br/>
TL;DR: `pip install` is on the way out. Installing Python packages must be done via APT, aka. Kali Linux’s package manager. Python packages coming from other sources should be installed in virtual environments.

Long story below.

Some background
---------------

Back in February this year, for a few days, some of you might have tried (and failed) to install Python packages with [Pip](https://pip.pypa.io/en/stable/), aka. Python’s package manager. Suddenly it didn’t work anymore, and it gave this error message instead:

```console
┌──(root㉿kali)-[~]
└─$ pip install xyz
error: externally-managed-environment
? This environment is externally managed
╰─> To install Python packages system-wide, try apt install
python3-xyz, where xyz is the package you are trying to
install.
If you wish to install a non-Debian-packaged Python package,
create a virtual environment using python3 -m venv path/to/venv.
Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
sure you have python3-full installed.
If you wish to install a non-Debian packaged Python application,
it may be easiest to use pipx install xyz, which will manage a
virtual environment for you. Make sure you have pipx installed.
See /usr/share/doc/python3.11/README.venv for more information.
note: If you believe this is a mistake, please contact your Python installation
or OS distribution provider. You can override this, at the risk of breaking
your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
```

This change came about without a notice, and judging by the early reports that we received, it was clear that it would impact many users. So we reverted it, and therefore `pip install` [still works in Kali Linux these days](https://www.kali.org/blog/kali-linux-2023-1-release/#python-updates--changes). But not for long: when Python 3.12 hits Kali (around end of 2023 or beginning of 2024), it will stop working, this time for good. There’s not much we can do about it, it’s an upstream change, we have to go with the flow.

So why this change? Running `pip install` as root, in order to install Python packages system-wide, has never been a great idea. In a Linux distribution such as Kali, Python packages are already installed and managed via _APT_. If you bring in another package manager (_pip_ in this case), it is likely to break packages and programs that were installed by APT, sooner or later. Then APT might break again what was installed by pip. Both package managers will endlessly step on each other’s toes.

One could also run `pip install --user` to install packages in the user’s home directory, but the problem is the same. Those packages will be picked up by Python applications as they run, and might not be compatible with other packages installed by _APT_, causing programs to misbehave or break.

The issue is not new, but it doesn’t impact all users equally. Seasoned users of Linux distributions already know what to do, and NOT to do, and they can fix their system when it breaks. However, unexperienced users don’t know, so they are likely to shoot themselves in the foot. And nobody can blame them, there are so many web pages out there recommending to run `sudo pip install` without providing enough context.

We (Kali developers, and more generally distro developers), are well aware of the issue: bug reports for Python applications that don’t work are a common occurence, and we often can’t reproduce the issue, and we often find out that it doesn’t work because some packages or applications were installed with _pip_, and interfere with other packages installed with _APT_. These recurring bug reports are not actionable, there’s nothing we can fix on our side. Users get burnt and they learn from it, but it’s no fun.

What’s changing
---------------

Now, back to the upcoming change: **in Kali Linux, starting with Python 3.12, pip will refuse to perform system-wide installs (`sudo pip install`) as well as user home directory installs (`pip install --user`)**. This is good news, because it will make it harder for unexperienced users to break their system. This is a welcome change, and we are thankful to those who drove this change and made it happen. Long-term, it will be less pain for everyone. But short-term, some users won’t like it, of course, we know.

So if you’re one of those who run `sudo pip install`, who have it hardwired in your fingers… well, you’ll have to adjust. You might want to have a look at [pipx](https://pypa.github.io/pipx/), get more familiar with Python’s virtual environments, and spend some time reading [PEP 668: Marking Python environments as externally managed](https://peps.python.org/pep-0668/) to better understand the issue at hand.

To finish, and to give a bit of a broader context: the PEP 668 proposal came about as a coordinated effort from various software distributions to fix this long-standing issue of _pip_ breaking other package managers too easily. The change is already effective in other Linux distros (like the latest release of Debian). In Kali Linux, we just delayed it a bit, so that we can warn you in advance, so that you can adjust your workflow and scripts. But the change is coming with Python 3.12, there’s no point delaying it further.

Thanks for reading!

#### [Source](https://www.kali.org/blog/python-externally-managed/)

<br/>
---
