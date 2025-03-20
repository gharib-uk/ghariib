---
title: "The DeepSeek controversy: Authorities ask where does the data come from and how safe is it?"
date: 2025-02-01
---

The sudden rise of DeepSeek has raised concerns and questions, especially about the origin and destination of the training data, as well as the security of the data.

For those returning from a short holiday away from the news, DeepSeek is a new player on the Artificial Intelligence (AI) field. The Chinese startup has certainly taken the app stores by storm: In just a week after the launch it topped the charts as the most downloaded free app in the US. This caused an upset on the stock markets that cost nVidia and Oracle shareholders a lot of money.

DeepSeek has been called an open-source project, however this technically is not true because only the model’s outputs and certain aspects are publicly accessible. This makes it qualify as an open-weight model. Anyway, the important difference is that the underlying training data and code necessary for full reproduction of the models are not fully disclosed.

And it’s the data that pose a concern to many. OpenAI has accused DeepSeek of using its ChatGPT model to train DeepSeek’s AI chatbot, which triggered quite some memes. If only because OpenAI previously suffered accusations of using data that was not its own in order to train ChatGPT.

![You're trying to kidnap what I've rightfully stolen](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/kidnapped_stolen_meme.png)

Authorities have started to ask questions as well. The Italian privacy regulator GPDP has asked DeepSeek to provide information about the data it processes in the chatbot, and its training data.  Because it sees a risk to the privacy of millions of Italian citizens, GDPD has demanded DeepSeek answers within 20 days questions about:

- Which personal data is collected

- The origin of the data

- Purpose for the collection

- Whether the data is stored on servers in China

According to the Italian press agency ANSA, DeepSeek disappeared on January 29, 2025 from Google and Apple’s app stores in Italy.

And if all that isn’t scary enough, researchers at Wiz have found a publicly accessible database belonging to DeepSeek.

> “This database contained a significant volume of chat history, backend data and sensitive information, including log streams, API Secrets, and operational details. “

The database was not just accessible and readable, it was also open to control and privilege escalation within the DeepSeek environment. No authentication was required, so anybody that stumbled over the database was able to run queries to retrieve sensitive logs and actual plaintext chat messages, and even to steal plaintext passwords and local files.

Needless to say, this oversight put DeepSeek and its users at risk.

We have said this before and we’ll probably have to repeat it numerous times, but the need for fast developments in this field is creating privacy risks that we have never seen before, simply because security is an afterthought for the developers. So, no matter which AI chatbot you prefer, always be mindful of the information you feed it: It may find its way to unexpected and undesirable places.

* * *

**We don’t just report on threats – we help safeguard your entire digital identity**

Cybersecurity risks should never spread beyond a headline. Protect your—and your family’s—personal information by using identity protection.

Go to Source
