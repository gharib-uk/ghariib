---
title: "Your Single-Page Applications Are Vulnerable Heres How to Fix Them"
date: Wed, 15 Jan 2025 14:00:00 +0000
draft: false
type: posts
categories: 
- Threat Intelligence
---
# Your Single-Page Applications Are Vulnerable Heres How to Fix Them

<br/>

<br/>
Written by: Steven Karschnia, Truman Brown, Jacob Paullus, Daniel McNamara

* * *

Executive Summary
-----------------

-   Due to their client-side nature, single-page applications (SPAs) will typically have multiple access control vulnerabilities
    
-   By implementing a robust access control policy on supporting APIs, the risks associated with client-side rendering can be largely mitigated
    
-   Using server-side rendering within the SPA can prevent unauthorized users from modifying or even viewing pages and data that they are not authorized to see
    

Introduction
------------

Single-page applications (SPAs) are popular due to their dynamic and user-friendly interfaces, but they can also introduce security risks. The client-side rendering frequently implemented in SPAs can make them vulnerable to unauthorized access and data manipulation. This blog post will explore the vulnerabilities inherent in SPAs, including routing manipulation, hidden element exposure, and JavaScript debugging, as well as provide recommendations on how to mitigate these risks.

Single-Page Applications
------------------------

A SPA is a web application design framework in which the application returns a single document whose content is hidden, displayed, or otherwise modified by JavaScript. This differs from the flat file application framework traditionally implemented in PHP or strictly HTML sites and from the Model-View-Controller (MVC) architecture where data, views, and server controls are handled by different portions of the application. Dynamic data in SPAs is updated through API calls, eliminating the need for page refreshes or navigation to different URLs. This approach makes SPAs feel more like native applications, offering a seamless user experience. JavaScript frameworks that are commonly used to implement SPAs include React, Angular, and Vue.

Client-Side Rendering
---------------------

In SPAs that use client-side rendering, a server responds to a request with an HTML document that contains only CSS, metadata, and JavaScript. The initially returned HTML document does not contain any content, and instead once the JavaScript files have been run in the browser, the application’s frontend user interface (UI) and content is loaded into the HTML document at runtime. If the application is designed to use routing, JavaScript takes the URL and attempts to generate the page that the user requested. While this is happening, the application is making requests to the API endpoint to load data and check whether or not the current user is authorized to access the data. If a user is not yet authenticated, then the application will render a login page or redirect the user to a separate single sign-on (SSO) application for authentication.

While all of this happens, a user may briefly observe a blank white page before the application dashboard or login page is loaded into their browser. During this pause, the application is potentially loading hundreds of thousands of lines of minified JavaScript that will build the full user experience of the application. SPAs are used in millions of applications across the globe, including Netflix, Hulu, Uber, and DoorDash.

Issues with Client-Side Rendering
---------------------------------

Because SPAs rely entirely on the client’s browser to render content (using API data), users have significant control over the application. This enables users to manipulate the application freely, making user or role impersonation easier.

### Routing

One fundamental aspect of the JavaScript frameworks that SPAs are implemented in is the idea of routes. These frameworks use routes to indicate different pages in the application. Routes in this case are different views that a user can see, like a dashboard or user profile. Since all of the JavaScript is handled by the client's browser, the client can view these routes in the JavaScript files that are included in the application source. If a user can identify these routes, they can attempt to access any of them. Depending on how the JavaScript was implemented, there may be checks in place to see if a user has access to the specific route. The following is an example of React routing that includes information on creating the views, and more importantly `path` attributes.

```
In = function () {
	return (0, _.jsx)(d.rs, {
		children: (0, _.jsxs)(ki, {
			children: [
			(0, _.jsx)(d.AW, {
				path: "/dashboard",
				children: (0, _.jsx)(Ii, {}),
			}),
			(0, _.jsx)(d.AW, {
				path: "/users",
				children: (0, _.jsx)(wi, {}),
			}),
			(0, _.jsx)(d.AW, {
				path: "/profile",
				children: (0, _.jsx)(Ti, {}),
			}),
			],
		}),
	});
};
```

### Hidden Elements

One way that access control is handled by SPAs is through hidden page elements. This means that when the page loads, the application checks the user's role through local/session storage, cookie values, or server responses. After the application checks the user’s role, it then displays or hides elements based on the user's role. In some cases, the application only renders elements that are accessible by the user. In other cases, the application renders every element but "hides" them by controlling the CSS properties of the element. Hidden elements can be exposed through browser Developer Tools, allowing users to force their display. These hidden elements could be form fields or even links to other pages.

### JavaScript Debugging

Modern browsers allow users to debug JavaScript in real time with breakpoints. Modern web browsers allow breakpoints to be set on JavaScript files, which can be used to modify variables or rewrite functions all together. Debugging core functions can allow users to bypass access controls and gain unauthorized page access. Consider the following JavaScript:

```
function isAuth() {
        var user;
        var cookies = document.cookies;
        var userData = btoa(cookies).split(‘:’);
        if (userData.length == 3) {
                user.name = userData[0];
                user.role = userData[1];
                user.isAuthed = userData[2]; 
        } else {
                user.name = “”;
                user.role = “”;
                user.isAuthed = false; 
        }
        return user;
}
```

The previously defined function reads a user’s cookie, Base64 decodes the value, splits the text using `:` as the delimiter, and if the values match, it considers the user as authenticated. Identifying these core functions allows an attacker to bypass any authorization and access controls that are being handled by the client-side application.

Exploitation
------------

Manually exploiting JavaScript framework issues takes time and practice, but there are a few techniques that can make it easier. A common technique involves analyzing JavaScript files to identify application routes. Identifying routes allows you to “force-browse” to application pages and access them directly, rather than through the UI. This technique may work on its own, but other times you may need to identify any role checks in the application. These checks can be accessed through the JavaScript debugger to modify variables during execution to bypass authorization or authentication checks. Another useful technique involves capturing server responses to requests for user information in an HTTP proxy, such as Burp Suite Professional, and manually modifying the user object. While these exploitation techniques are effective, they can be mitigated through strong preventative measures, including those detailed in this post.

Recommendations
---------------

Access control issues are systemic to client-side-rendered JavaScript frameworks. Once a user has the application loaded into their browser, there are few effective mitigations to prevent the user from interacting with content in unauthorized ways. However, by implementing robust server-side access control checks on APIs, the effect that an attacker could produce is severely reduced. While the attacker might be able to view what a page would look like in the context of an administrator or even view the structure of a privileged request, the attacker would be unable to obtain or modify restricted data. 

API requests should be logged and monitored to identify if unauthorized users are attempting to or successfully accessing protected data. Additionally, it is advisable to conduct periodic penetration tests of web applications and APIs throughout their lifetime to identify any gaps in security. Penetration testing should uncover any APIs with partial or incomplete access control implementations, which would provide an opportunity to remediate flaws before they are abused by an adversary.

### API Access Controls

Implementing robust API access controls is critical for securing SPAs. Access control mechanisms should use a JSON Web Token (JWT) or other unique, immutable session identifier to prevent users from modifying or forging session tokens. API endpoints should validate session tokens and enforce role-based access for every interaction. APIs are often configured to check if a user is authenticated, but they don’t comprehensively check user role access to an endpoint. In some cases, just one misconfigured endpoint is all it takes to compromise an application. For example, if all application endpoints are checking a user’s role except the admin endpoint that creates new users, then an attacker can create users at arbitrary role levels, including admin users. 

An example of proper API access control is shown in Figure 1.

![Proper API access control example](https://storage.googleapis.com/gweb-cloudblog-publish/images/spa-fig1.max-1000x1000.png)

Figure 1: Proper API access control example

This diagram shows a user authenticating to the application, receiving a JWT, and rendering a page. The user interacts with the SPA and requests a page. The SPA identifies that the user is not authenticated so the JavaScript renders the login page. Once a user submits the login request, the SPA forwards it to the server through an API request. The API responds stating the user is authenticated and provides a JWT that can be used with subsequent requests. Once the SPA receives the response from the server, it stores the JWT and renders the dashboard that the user originally requested. 

At the same time, the SPA requests the data necessary to render the page from the API. The API sends the data back to the application, and it is displayed to the user. Next, the user finds a way to bypass the client-side access controls and requests the main admin page in the application. The SPA makes the API requests to render the data for the admin page. The backend server checks the user’s role level, but since the user is not an admin user, the server returns a 403 error stating that the user is not allowed to access the data.

The example in Figure 1 shows how API access controls prevent a user from accessing API data. As stated in the example, the user was able to access the page in the SPA; however, due to the API access controls, they are not able to access the data necessary to fully render the page. For APIs developed in C# or Java, frameworks often provide annotations to simplify implementing access controls.

### Server-Side Rendering

Aside from API access controls, another way to mitigate this issue is by using a JavaScript framework that has server-side rendering capabilities, such as Svelte-Kit, Next.js, Nuxt.js, or Gatsby. Server-side rendering is a combination of the MVC and SPA architectures. Instead of delivering all source content at once, the server renders the requested SPA page and sends only the finalized output to the user. The client browser is no longer in charge of routing, rendering, or access controls. The server can enforce access control rules before rendering the HTML, ensuring only authorized users see specific components or data.

An example of server-side rendering is shown in Figure 2.

![Server-side rendering example](https://storage.googleapis.com/gweb-cloudblog-publish/images/spa-fig2.max-1000x1000.png)

Figure 2: Server-side rendering example

This diagram shows a user accessing a server-side rendered application. After requesting an authenticated page in the application, the server checks if the user is authenticated and authorized to view the page. Since the user is not yet authenticated, the application renders the login page and displays that page to the user. The user then authenticates, and the server builds out the session, sets necessary cookies or tokens, and then redirects the user to the application dashboard. Upon being redirected, the user makes a request, the server checks the authentication state, and since the user has permissions to access the page, it fetches the necessary data and renders the dashboard with the data. 

Next, the user identifies an admin page URL and attempts to access it. In this instance, the application checks the authentication state and the user’s role. Since the user does not have the admin role, they are not allowed to view the page and the server responds with either a `403 Forbidden` or a redirection to an error page.

A Final Word
------------

In conclusion, SPAs offer a dynamic and engaging user experience, but they also introduce unique security challenges when implemented with client-side rendering. By understanding the vulnerabilities inherent in SPAs, such as routing manipulation, hidden element exposure, and JavaScript debugging, developers can take proactive steps to mitigate risks. Implementing robust server-side access controls, API security measures, and server-side rendering are excellent ways to safeguard SPAs against unauthorized access and data breaches. Regular penetration testing and security assessments can further strengthen the overall security posture of SPAs by identifying any security gaps present in the application and allowing developers to remediate them before they are exploited. By prioritizing security best practices, developers can ensure that SPAs deliver both a seamless user experience and a secure environment for sensitive data.

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/single-page-applications-vulnerable/)

<br/>
---
