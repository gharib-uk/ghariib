---
title: "Building a Puppeteer Web Scraper with Docker on DigitalOcean App Platform"
date: 2025-01-10T19:48:39.525Z
draft: false
type: posts
categories: 
- development,docker,data-analysis
---
# Building a Puppeteer Web Scraper with Docker on DigitalOcean App Platform

<br/>

<br/>
As an ultra marathon enthusiast, I often face a common challenge: how do I estimate my finish time for longer races I haven’t attempted yet? When discussing this with my coach, he suggested a practical approach—look at runners who’ve completed both a race I’ve done and the race I’m targeting. This correlation could provide valuable insights into potential finish times. But manually searching through race results would be incredibly time-consuming.

This led me to build [Race Time Insights](https://www.racetimeinsights.com/), a tool that automatically compares race results by finding athletes who’ve completed both events. The application scrapes race results from platforms like UltraSignup and Pacific Multisports, allowing runners to input two race URLs and see how other athletes performed across both events.

Building this tool showed me just how powerful [DigitalOcean’s App Platform](/products/app-platform) could be. Using Puppeteer with headless Chrome in Docker containers, I could focus on solving the problem for runners while App Platform handled all the infrastructure complexity. The result was a robust, scalable solution that helps the running community make data-driven decisions about their race goals.

After building Race Time Insights, I wanted to create a guide showing other developers how to leverage these same technologies—Puppeteer, Docker containers, and DigitalOcean App Platform. Of course, when working with external data, you need to be mindful of things like rate limiting and terms of service.

Enter Project Gutenberg. With its vast collection of public domain books and clear terms of service, it’s an ideal candidate for demonstrating these technologies. In this post, we’ll explore how to build a book search application using Puppeteer in a Docker container, deployed on App Platform, while following best practices for external data access.

[Project Gutenberg Book Search](#project-gutenberg-book-search)[](#project-gutenberg-book-search)
-------------------------------------------------------------------------------------------------

I’ve built and shared a web application that responsibly scrapes book information from Project Gutenberg. The app, which you can find in this [GitHub repository](https://github.com/wadewegner/doappplat-puppeteer-sample), allows users to search through thousands of public domain books, view detailed information about each book, and access various download formats. What makes this particularly interesting is how it demonstrates responsible web scraping practices while providing genuine value to users.

[Being a Good Digital Citizen](#being-a-good-digital-citizen)[](#being-a-good-digital-citizen)
----------------------------------------------------------------------------------------------

When building a web scraper, it’s crucial to follow good practices and respect both technical and legal boundaries. Project Gutenberg is an excellent example for learning these principles because:

1.  It has clear terms of service
2.  It provides robots.txt guidelines
3.  Its content is explicitly in the public domain
4.  It benefits from increased accessibility to its resources

Our implementation includes several best practices:

### [Rate Limiting](#rate-limiting)[](#rate-limiting)

For demonstration purposes, we implement a simple rate limiter that ensures at least 1 second between requests:

```
// A naive rate limiting implementation
const rateLimiter = {
    lastRequest: 0,
    minDelay: 1000, // 1 second between requests
    async wait() {
        const now = Date.now();
        const timeToWait = Math.max(0, this.lastRequest + this.minDelay - now);
        if (timeToWait > 0) {
            await new Promise(resolve => setTimeout(resolve, timeToWait));
        }
        this.lastRequest = Date.now();
    }
};
```

This implementation is intentionally simplified for the example. It assumes a single application instance and stores state in memory, which wouldn’t be suitable for production use. More robust solutions might use Redis for distributed rate limiting or implement queue-based systems for better scalability.

This rate limiter is used before every request to Project Gutenberg:

```
async searchBooks(query, page = 1) {
    await this.initialize();
    await rateLimiter.wait();  // Enforce rate limit
    // ... rest of search logic
}

async getBookDetails(bookUrl) {
    await this.initialize();
    await rateLimiter.wait();  // Enforce rate limit
    // ... rest of details logic
}
```

### [Clear Bot Identification](#clear-bot-identification)[](#clear-bot-identification)

A custom User-Agent helps website administrators understand who is accessing their site and why. This transparency allows them to:

1.  Contact you if there are issues
2.  Monitor and analyze bot traffic separately from human users
3.  Potentially provide better access or support for legitimate scrapers

```
await browserPage.setUserAgent('GutenbergScraper/1.0 (Educational Project)');
```

### [Efficient Resource Management](#efficient-resource-management)[](#efficient-resource-management)

Chrome can be memory-intensive, especially when running multiple instances. Properly closing browser pages after use prevents memory leaks and ensures your application runs efficiently, even when handling many requests:

```
try {
    // ... scraping logic
} finally {
    await browserPage.close();  // Free up memory and system resources
}
```

By following these practices, we create a scraper that’s both effective and respectful of the resources it accesses. This is particularly important when working with valuable public resources like Project Gutenberg.

[Web Scraping in the Cloud](#web-scraping-in-the-cloud)[](#web-scraping-in-the-cloud)
-------------------------------------------------------------------------------------

The application leverages modern cloud architecture and containerization through [DigitalOcean’s App Platform](/products/app-platform). This approach provides a perfect balance between development simplicity and production reliability.

### [The Power of App Platform](#the-power-of-app-platform)[](#the-power-of-app-platform)

App Platform streamlines the deployment process by handling:

-   Web server configuration
-   SSL certificate management
-   Security updates
-   Load balancing
-   Resource monitoring

This allows us to focus on the application code while App Platform manages the infrastructure.

### [Headless Chrome in a Container](#headless-chrome-in-a-container)[](#headless-chrome-in-a-container)

The core of our scraping functionality uses [Puppeteer](https://pptr.dev/), which provides a high-level API to control Chrome programmatically. Here’s how we set up and use Puppeteer in our application:

```
const puppeteer = require('puppeteer');

class BookService {
    constructor() {
        this.baseUrl = 'https://www.gutenberg.org';
        this.browser = null;
    }

    async initialize() {
        if (!this.browser) {
            // Add environment info logging for debugging
            console.log('Environment details:', {
                PUPPETEER_EXECUTABLE_PATH: process.env.PUPPETEER_EXECUTABLE_PATH,
                CHROME_PATH: process.env.CHROME_PATH,
                NODE_ENV: process.env.NODE_ENV
            });

            const options = {
                headless: 'new',
                args: [
                    '--no-sandbox',
                    '--disable-setuid-sandbox',
                    '--disable-dev-shm-usage',
                    '--disable-gpu',
                    '--disable-extensions',
                    '--disable-software-rasterizer',
                    '--window-size=1280,800',
                    '--user-agent=GutenbergScraper/1.0 (+https://github.com/wadewegner/doappplat-puppeteer-sample) Chromium/120.0.0.0'
                ],
                executablePath: process.env.PUPPETEER_EXECUTABLE_PATH || '/usr/bin/chromium-browser',
                defaultViewport: {
                    width: 1280,
                    height: 800
                }
            };

            this.browser = await puppeteer.launch(options);
        }
    }

    // Example of scraping with Puppeteer
    async searchBooks(query, page = 1) {
        await this.initialize();
        await rateLimiter.wait();

        const browserPage = await this.browser.newPage();
        try {
            // Set headers to mimic a real browser and identify our bot
            await browserPage.setExtraHTTPHeaders({
                'Accept-Language': 'en-US,en;q=0.9',
                'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
                'Connection': 'keep-alive',
                'Upgrade-Insecure-Requests': '1',
                'X-Bot-Info': 'GutenbergScraper - A tool for searching Project Gutenberg'
            });

            const searchUrl = `${this.baseUrl}/ebooks/search/?query=${encodeURIComponent(query)}&start_index=${(page - 1) * 24}`;
            await browserPage.goto(searchUrl, { waitUntil: 'networkidle0' });
            
            // ... rest of search logic
        } finally {
            await browserPage.close();  // Always clean up
        }
    }
}
```

This setup allows us to:

-   Run Chrome in headless mode (no GUI needed)
-   Execute JavaScript in the context of web pages
-   Safely manage browser resources
-   Work reliably in a containerized environment

The setup also includes several important configurations for running in a containerized environment:

1.  **Proper Chrome Arguments**: Essential flags like `--no-sandbox` and `--disable-dev-shm-usage` for running in containers
2.  **Environment-aware Path**: Uses the correct Chrome binary path from environment variables
3.  **Resource Management**: Sets viewport size and disables unnecessary features
4.  **Professional Bot Identity**: Clear user agent and HTTP headers identifying our scraper
5.  **Error Handling**: Proper cleanup of browser pages to prevent memory leaks

While Puppeteer makes it easy to control Chrome programmatically, running it in a container requires proper system dependencies and configuration. Let’s look at how we set this up in our Docker environment.

### [Docker: Ensuring Consistent Environments](#docker-ensuring-consistent-environments)[](#docker-ensuring-consistent-environments)

One of the biggest challenges in deploying web scrapers is ensuring they work the same way in development and production. Your scraper might work perfectly on your local machine but fail in the cloud due to missing dependencies or different system configurations. Docker solves this by packaging everything the application needs - from Node.js to Chrome itself - into a single container that runs identically everywhere.

Our Dockerfile sets up this consistent environment:

```
FROM node:18-alpine

# Install Chromium and dependencies
RUN apk add --no-cache \
    chromium \
    nss \
    freetype \
    harfbuzz \
    ca-certificates \
    ttf-freefont \
    dumb-init

# Set environment variables
ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true \
    PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser \
    PUPPETEER_DISABLE_DEV_SHM_USAGE=true
```

The Alpine-based image keeps our container lightweight while including all necessary dependencies. When you run this container, whether on your laptop or in DigitalOcean’s App Platform, you get the exact same environment with all the right versions and configurations for running headless Chrome.

[Development to Deployment](#development-to-deployment)[](#development-to-deployment)
-------------------------------------------------------------------------------------

Let’s walk through getting this project up and running:

### [1\. Local Development](#1-local-development)[](#1-local-development)

First, fork the [example repository](https://github.com/wadewegner/doappplat-puppeteer-sample) to your GitHub account. This gives you your own copy to work with and deploy from. Then clone your fork locally:

```
# Clone your fork
git clone https://github.com/YOUR-USERNAME/doappplat-puppeteer-sample.git
cd doappplat-puppeteer-sample

# Build and run with Docker
docker build -t gutenberg-scraper .
docker run -p 8080:8080 gutenberg-scraper
```

### [2\. Understanding the Code](#2-understanding-the-code)[](#2-understanding-the-code)

The application is structure around three main components:

1.  **Book Service**: Handles web scraping and data extraction
    
    ```
    async searchBooks(query, page = 1) {
     await this.initialize();
     await rateLimiter.wait();
    
     const itemsPerPage = 24;
     const searchUrl = `${this.baseUrl}/ebooks/search/?query=${encodeURIComponent(query)}&start_index=${(page - 1) * itemsPerPage}`;
     
     // ... scraping logic
    }
    ```
    
2.  **Express Server**: Manages routes and renders templates
    
    ```
    app.get('/book/:url(*)', async (req, res) => {
     try {
         const bookUrl = req.params.url;
         const bookDetails = await bookService.getBookDetails(bookUrl);
         res.render('book', { book: bookDetails, error: null });
     } catch (error) {
         // Error handling
     }
    });
    ```
    
3.  **Frontend Views**: Clean, responsive UI using Bootstrap
    
    ```
    <div class="card book-card h-100">
     <div class="card-body">
         <span class="badge bg-secondary downloads-badge">
             <%= book.downloads.toLocaleString() %> downloads
         </span>
         <h5 class="card-title"><%= book.title %></h5>
         <!-- ... more UI elements ... -->
     </div>
    </div>
    ```
    

### [3\. Deployment to DigitalOcean](#3-deployment-to-digitalocean)[](#3-deployment-to-digitalocean)

Now that you have your fork of the repository, deploying to DigitalOcean App Platform is straightforward:

1.  Create a new App Platform application
2.  Connect to your forked rep
3.  On resources, delete the second resource (that isn’t a Dockerfile); this is auto-generated by App Platform and not needed
4.  Deploy by clicking Create Resources

The application will be automatically built and deployed, with App Platform handling all the infrastructure details.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

This Project Gutenberg scraper demonstrates how to build a practical web application using modern cloud technologies. By combining Puppeteer for web scraping, Docker for containerization, and DigitalOcean’s App Platform for deployment, we’ve created a solution that’s both robust and easy to maintain.

The project serves as a template for your own web scraping applications, showing how to handle browser automation, manage resources efficiently, and deploy to the cloud. Whether you’re building a data collection tool or just learning about containerized applications, this example provides a solid foundation to build upon.

Check out the [project on GitHub](https://github.com/wadewegner/doappplat-puppeteer-sample) to learn more and deploy your own instance!

#### [Source](https://www.digitalocean.com/community/tutorials/build-a-puppeteer-web-scrapper-with-docker-and-app-platform)

<br/>
---
