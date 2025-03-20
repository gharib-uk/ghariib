---
title: "Creating A Custom View for WebSocket in ZAP"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
tags: 
  - "pentesting"
  - "plugin"
  - "websocket"
  - "zap"
---

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjbETbc1fXDey2o7NC3ZLEq1k5hZCqEoEpz4lzxWVtKG6ansPGEuxnVY7LV1Xm_5A3G8-jtvQgT16MtRSi-_y3AdyshZ_wi7yGDHzP8yn0a0iwo-k3z1JEDsPAHxie17ohTSnkf-uv-s1UD/s320/websocket-in-zap-image-feature%255B1%255D.png)

When we were looking at the interactions between the Outlook and the LinkedIn APIs, we encountered WebSocket communications that used some additional encoding. The encoding was nothing too complex, but it was uncommon. It turned out to be LZip compression. However, the inability to read the content of the requests with Burp, ZAP or Web developer consoles in real-time made it difficult to analyze the API.

While our proxy of choice is usually Burp Suite, it did not allow us to extend WebSocket views. We turn ourselves to the open-source project Zed Attack Proxy. It reveals to be easily extendable for custom WebSocket tooling. In this blog post, we will explain how you can implement your own custom view to display complex WebSocket messages.

## Building your custom ZAP WebSocket plugin

You will need three classes to link your decoding algorithm to ZAP. Those classes will implement the following Interfaces:

- **Extension:** This class will be the main class that is registered to ZAP to handle load (hook in ZAP’s terminology) and unload events when your plugin is activated or deactivated.
- **HttpPanelView:** This is where you will place your decoding algorithms and the UI components required to display the final value. You can always decouple the decoding code in separate classes to follow good design practices.
- **HttpPanelViewFactory:** This class handles the creation of new `HttpPanelView`. Little to no code will be needed.

At high-level, we are not only interacting with ZAP core. The WebSocket component is not part of ZAP core. This means two things. First, you will be depending on interface and class from the WebSocket plugin. You also need to declare in your plugin configuration that you are depending on an external plugin.

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEirZkDkuyOSz-Rxk19ZXqSP5PLE66nT1gqyCNN9ncDeasVgFK3A96bPDQ_oSkOhgRUuHdDUKeR8vK3F7kU0G8EFLHVk6z3vGlygAu11SbLLeRe8vscVvPK4RwqjlYX10Rx5xwWYO6kkr-Rv/w400-h210/websocket-in-zap-image-1%255B1%255D.png) |
| --- |
| High-level view of the dependencies |

  

  

### Registering your view

  

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjl3NNPTXKG7JwSke-kXiI0UqQYxKdGLMRii6t2a3pfIzPyO7OHPhOdqHrPjn7r7IWwBhVAw1mXIwXbacl-o0ySwdfZMmhoQNI7_xGfYoo8EArdCaQyFX26Cr7u4KwPMSmlfWpaEStwdoiM/w640-h454/websocket-in-zap-image-2%255B1%255D.png)

  
  

`HttpPanelView` is the interface that defines a view for HTTP messages and it is reused for the WebSocket traffic. In your extension initialization phase, you need to register your view. The only difference with HTTP messages will be the target component. In this case we must use “`WebSocketComponent`” (`WebSocketComponent.NAME`). Also note that the view is not registered directly. The Abstract Factory design pattern is used as follows:

```javascript
val viewFactory = AutoDecodeViewFactory()
panelManager.addRequestViewFactory(WebSocketComponent.NAME, viewFactory)
panelManager.addResponseViewFactory(WebSocketComponent.NAME, viewFactory)
```

### Implementing your view

The view will include the UI components required to display the final value. You can use basic Swing components or RSyntaxTextArea if you need some code highlight.

Here are the key functions to implement in an implementation of HttpPanelView:

1. The function getCaptionName() will define the caption for the view selection.

```javascript
override fun getCaptionName(): String {
        return "Auto-Decode"
    }
```

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjCbhUO9VKeAUzRnnDA5G9IP62QVAUo1agL6h99bZlUQhwTSePL1lxk7AeNyi2uQoJ8ZMsyMuJm9JnWCCAhWScfYr2bos525ObHx51iB5WGDZQ40sGxeuIsIEcW9K01yvzRXedp08jeZQtH/s320/websocket-in-zap-image-3%255B1%255D.png) |
| --- |
| Name being displayed when switching view |

  

  

2. `getPane()` is retuning a reference to the swing root component. It is recommended to build your layout once and always return the same reference for better performance.

```javascript
override fun getPane(): JComponent {
        return mySwingComponent
    }
```

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhwAsD-0jqcurk6OhshpBrwgHYWOzHWrT5nkL2EAsL1FbqC9vwjCkp1RAP49Y1NBSGKLulF0AeoFbuTq6_zIgh_MlnXEhT_gMBI5y5A-IyoLoGMCxumluNtTNdLlah5-r3mYlM8_Lv4vjTd/s320/websocket-in-zap-image-4%255B1%255D.png) |
| --- |
| The section is the customizable part |

  

3. `dataChanged()` is the function called when a new message is selected and your view is active. When handling this event, you will need to update your component to display.

```javascript
override fun dataChanged(event: HttpPanelViewModelEvent) {
    ...
}
```

### The final product

Here is the plugin that was built to handle multiple generic encoding or compression algorithms. For each incoming or outgoing message, we are testing a variety of common encoding and compression algorithms. In order, we are testing the following algorithms. If one succeeds, we are displaying its result.

- ZIP
- LZIP
- Base64
- URL decoding
- Brotli

We are displaying the same message in hexadecimal and in text format to support both text message and binary format.

Here are two screenshots showing the final product in action.

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEizBUPj5KWEmTfnZePWSilArfb-bvEUAlV6QpYJd8-K5_UMkhkowfZNlDfpbc5iBGgBEprJcNF21zNB3uM4oR0BBnhc66IXkerdUhcbpzmLZ3S9JxTW1m8HUYNXKDAsp-Gz5Gzjkx3Mgkc9/w640-h478/websocket-in-zap-image-5%255B1%255D.png) |
| --- |
| The Decoded tab is selected showing the decompressed message |

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjVdlSwGCpOrOQe3MTIDSUH7I4CUb64u2FCcR43BKF-mSScGGteOQRuSU4JXg1O7hchJzLtfW44K4PJZzzhH98GI1oaeG876CUoCf9NWIZY9yE7KHxltGjJzILJtYV-XXw0GmVwwlW40Kik/w640-h478/websocket-in-zap-image-6%255B1%255D.png) |
| --- |
| A second tab is showing the original LZip compressed message |

  

You can view the code on GoSecure’s GitHub repository at https://github.com/GoSecure/zap-autodecode-view.

## Conclusion

We hope this blog will be useful for security tester or developer that wants to analyze WebSocket communication with ease. The plugin we built might be useful for other applications that compresses messages. If you encounter a protocol that does not use readable messages, you will be able to implement your own view.

## References

Official ZAP website: https://www.zaproxy.org/

  

  

  

_This blog was originally posted on GoSecure blog._

Go to Source
