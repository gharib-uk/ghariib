---
title: "How to Install OpenJDK JDK 24 in Ubuntu 24.04 | 22.04"
date: 2025-03-19
---

![](https://ubuntuhandbook.org/wp-content/uploads/2022/03/java-logo-250x250.png)

OpenJDK announced the latest JDK 24 yesterday. This is the beginner’s guide shows how to install it in all current Ubuntu and Linux Mint releases.

OpenJDK 24 is a short term release with 6 months support. Ubuntu has made JDK 24 into system repository for upcoming Ubuntu 25.04, while all current Ubuntu releases may use the official tarball instead.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/rc24-java24-700x366.webp)

image from oracle.com

Ubuntu developers provide updates for the long term support (LTS) OpenJDK releases (e.g., JDK 8, JDK 17, and JDK 21). Users can install them simply by running a single command in terminal.

For example, you may install OpenJDK 21 in all current Ubuntu releases by running the single command below in terminal:

```
sudo apt install openjdk-21-jdk
```

While you may replace **21** with 17 or 8 for other JDK LTS releases.

For non-LTS releases, they also maintains a restricted PPA that contains the most recent OpenJDK for all current Ubuntu releases. However, it’s not updated for JDK 24 at the moment of writing.

### Difference between OpenJDK and Oracle JDK

OpenJDK and Oracle JDK are “same thing”, but with different licence.

Oracle JDK is built from the OpenJDK JDK source, but non-opensource licence. While both OpenJDK source and the OpenJDK builds binaries are distributed under the same GPL2+CPE licence.

**Both are free!** Though, Oracle also offers a subscription for the Oracle JDK builds that user can buy for support! And, I’ve written about how to install Oracle Java 21/24 in Ubuntu.

### Install OpenJDK JDK 24:

**NOTE: The steps below will override Oracle JDK 24 if installed.**

**1.** To install the new OpenJDK JDK 24, first go to java website via the link below:

Download OpenJDK 24

Then select download the “Linux / x64” build for AMD/Intel platform, or “Linux / AArch64” for ARM devices (e.g., Raspberry Pi).

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/download-openjdk24-700x507.webp)

**2.** After downloaded the Linux tarball, press `Ctrl+Alt+T` to open up a terminal window, and run commands below to extract to `/usr/lib/jvm` directory.

- First, create the target directory in case this is the first time you install Java on the system.
    
    ```
    sudo mkdir -p /usr/lib/jvm
    ```
    
- Then, run command to extract OpenJDK 24 tarball into that directory:
    
    ```
    sudo tar -zxf ~/Downloads/openjdk-24_linux-*_bin.tar.gz -C /usr/lib/jvm/
    ```
    
    Here assume you saved the tarball in user Downloads folder.
    

After that, run `ls /usr/lib/jvm/` to list directory content. It should include a new **jdk-24** (or jdk-24.0.1, jdk-24.0.2, etc) sub-folder.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/extract-openjdk24-700x385.webp)

**3\. Set OpenJDK JDK 24 as default**.

To set it as default, first run commands below one by one to link executable files as alternatives:

```
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-24/bin/java 1
```

```
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk-24/bin/javac 1
```

```
sudo update-alternatives --install /usr/bin/jar jar /usr/lib/jvm/jdk-24/bin/jar 1
```

As you see in the screenshot below, there are many other executable files under `/usr/lib/jvm/jdk-24/bin` (e.g., javap, jdb, jfr, and more). You may run similar commands above for them one by one as you need.

**NOTE: As time goes on, OpenJDK may release updates for JDK 24. In the case, you’ll need to replace “jdk-24” in command with “jdk-24.0.1” or “jdk-24.0.2” according to the Java root folder name under “/usr/lib/jvm”.**

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/add-openjdk24-alternatives-700x364.webp)

After that, run the commands below one by one to choose default Java:

```
sudo update-alternatives --config java
```

```
sudo update-alternatives --config javac
```

```
sudo update-alternatives --config jar
```

Also run similar command above for other executable files if you created links for them.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/config-openjdk24-700x456.webp)

**4.** Finally, run command to check the default Java edition:

```
java --version
```

```
javac --version
```

And, use `java -XshowSettings:properties -version` to print more the properties.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/verify-openjdk24-700x505.webp)

### Uninstall OpenJDK JDK 24

To uninstall OpenJDK JDK 24 installed via the steps above, simply open terminal (Ctrl+Alt+T) and run command to remove the Java folder under `/usr/lib/jvm`:

```
sudo rm -R /usr/lib/jvm/jdk-24
```

Then, remove the alternative links:

```
sudo update-alternatives --remove java /usr/lib/jvm/jdk-24/bin/java
```

```
sudo update-alternatives --remove javac /usr/lib/jvm/jdk-24/bin/javac
```

```
sudo update-alternatives --remove jar /usr/lib/jvm/jdk-24/bin/jar
```

Also run similar command above for other executable files if added. And, replace **jdk-24** with **jdk-24.0.1** etc. for point releases.

Go to Source
