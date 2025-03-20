---
title: "Fundamentals of HTML and CSS"
date: 2025-01-06
---

HTML and CSS are the most fundamental building blocks of a webpage, and they are also your first step towards becoming a web developer. HTML provides the layout and content of the webpage, and CSS defines its style and appearance. In this tutorial, we are going to cover the basics of HTML and CSS, and by the end, you will be able to design responsive webpages that work seamlessly on devices of all sizes.

## What is HTML?

HTML is the standard markup language used to create webpages. It defines the structure and content of webpages using HTML elements such as headings, paragraphs, images, links, forms, and more.

To start writing HTML code, you can use the CodePen demo below:

Code Demo 🔗

On the left side, you will find the HTML source code, which is essentially the blueprint for what will be displayed. The browser will then transform this blueprint into the webpage you see on the right side.

You can modify the source code directly to see how it affects the displayed webpage.

## Prepare your computer for web development

Of course, in practice, you cannot rely on tools such as CodePen to create a working and fully featured web application. You need something more powerful, so, let's set up your computer for web development.

To get started, make sure you have a browser installed. Any popular web browser such as Google Chrome, Microsoft Edge, Safari, or Firefox, should be sufficient for this course. You may download and install the browser from the linked websites.

In addition, you'll need a code editor to write and edit your code. Visual Studio Code is a great option for beginners (and professionals, for that matter). It is the most commonly used code editor in the world. Simply download the appropriate installer for your operating system from their official website.

![download vscode](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fr8vorpog341evojostzm.png)

After you've installed VSCode, make sure to install the **Live Server** extension as well. Navigate to the **Extensions** tab on the left sidebar, and type in **Live Server** in the search box. From there, you'll be able to download and install the extension.

![install live server](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9gstk1ypx6y5ohytq34i.png)

**Live Server** will create a local development server with the auto-reload feature. For example, create a new work directory and open it using VSCode.

![empty directory](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fsdbyf59qr6d3xufhfh34.png)

Create a new file named `index.html` under this directory. The `.html` extension indicates that this is an HTML document. Type in `!` in VSCode, and you will see suggestions like this:

![new html doc](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4y8d6xcuh61zt10y6x9v.png)

This is a shortcut for creating HTML documents quickly. You can navigate with the ↑ or ↓ keys. Select the first option, and the following code should be generated.  

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body></body>
</html>
```

Notice that at the bottom right corner of the VSCode window, there is a **Go Live** button.

![go live button](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxaz2413jt6kdx3kcze4x.png)

Clicking this button will activate the **Live Server** extension. A local development server will be started, hosting the `index.html` file you just created.

![empty html](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fwao4ms5e0fl9iwzkr9ye.png)

Of course, the file is still empty right now, so you can't see anything. Add something between the `<body>` and `</body>` tags.  

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    Hello, world!
  </body>
</html>
```

Save the changes, and the webpage will be automatically refreshed with the new content.

![html with hello world](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Foe0k4grdd5ui3ibi4ur3.png)

## The structure of an HTML document

A typical HTML document always has the following structure:  

```
<!DOCTYPE html>
<html lang="en">
  <head>
    . . .
  </head>
  <body>
    . . .
  </body>
</html>
```

The `<!DOCTYPE html>` tag defines the document type. When the web browser encounters `<!DOCTYPE html>`, it understands that the page should be parsed and displayed according to the specifications of HTML5, the latest version of HTML standard. This ensures that modern browsers interpret the webpage's content and layout correctly.

Everything else in the file should be enclosed inside an `<html>` element, defined by an opening tag (`<html>`) and a closing tag (`</html>`).

`lang` is called an attribute, and it has the value `"en"`. This tells the browser as well as the search engine that English is the primary language used for this webpage.

Inside the `<html>` element, there are two child elements, `<head>` and `<body>`. `<head>` contains metadata and other information about the HTML document. This information will not be displayed in the browser but is often used by search engines for SEO (Search Engine Optimization) purposes.

`<body>`, on the other hand, contains the main content of the webpage that is visible to the users, and for that reason, it is also the part of the HTML file we are going to focus on in this course.

## Elements and attributes

Let's take a closer look at the previous example and notice that the HTML document comprises different elements in a nested structure. In HTML, most elements have both an opening tag and a closing tag:  

```
<tag>. . .</tag>
```

In this case, `<tag>` is the opening tag, and `</tag>` is the closing tag, and together, they form an HTML element. An element could hold textual content between the opening and closing tags.  

```
<tag>Hello, world!</tag>
```

The element can also wrap around other elements, forming a nested structure.  

```
<tag>
  <tag>. . .</tag>
  <tag>
    <tag>. . .</tag>
  </tag>
</tag>
```

Inside the opening tag, you can define attributes, which are used to specify additional information about the element, such as its `class`, `id`, and so on.  

```
<tag attribute="value">. . .</tag>
```

The attribute is usually in a key/value pair, and the value must always be enclosed inside matching quotes (double or single).

There are some exceptions to these general formats. For example, the `<br>` element, which creates a line break, does not need a closing tag. Some attributes, such as `multiple`, do not require a value. We will discuss these exceptions later in this course as we encounter them.

However, you should remember that if an element does require a closing tag, then it should never be left out. In most cases, the webpage could still render correctly, but as the structure of your HTML document grows more complex, unexpected errors may occur. Take a look at our best practice guidelines for writing HTML and CSS if you are interested.

## Further readings

- Introducing the Cascading Style Sheet (CSS)
- Introduction to JavaScript
- What is Responsive Design
- How to Build Interactive Forms Using HTML and CSS

Go to Source
