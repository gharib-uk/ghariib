---
title: "Building an offline-available Progressive Web App News Aggregator using Vite and Vue"
date: 2025-01-20T00:00:00.000Z
draft: false
type: posts
categories: 
- development,vue-js,javascript
---
# Building an offline-available Progressive Web App News Aggregator using Vite and Vue

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

A Progressive Web App (PWA) is a mobile-friendly web application that offers native-like features and improved user experiences. Key benefits include the ability to install the app on any device and access it offline. Essential components of a PWA are a web app manifest and a service worker. The [IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) can be utilized for persisting structured data.

In this tutorial, you will convert a sample website build using [Vue](https://vuejs.org/) and [Vite](https://vite.dev/) to an offline-available PWA that end users can install on any device. You will create a web app manifest and a service worker to cache the presentation layer of the web app.

You will also use [IndexDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Using_IndexedDB) to store structured data about the app so that the data is available even while offline.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

To follow along with this tutorial, you will need the following:

-   A [Node.js development environment](/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-20-04) with Node version 18.15 or greater and npm version 9 or greater
-   A [git installation](/community/tutorials/how-to-install-git-on-ubuntu) in your dev environment
-   Some experience developing with [Vue](https://vuejs.org/) and [Vite](https://vite.dev/).
-   A [News API](https://newsapi.org/) account and API key. You can [register and get one here](https://newsapi.org/register).

[Step 1 - Setting up the project](#step-1-setting-up-the-project)[](#step-1-setting-up-the-project)
---------------------------------------------------------------------------------------------------

In this step, you will clone and set up the sample project in your local development environment. It uses Vite with VueJS for a faster and leaner development experience and Tailwind CSS for styling.

To start, you will clone an already-built Vue app for this tutorial from GitHub. You will be working with the What’s New app - a news aggregator that sources data from the News API, and categorizes and presents them as headlines, general news items and a personalized customizable news feed.

To clone the project from GitHub, first, you need to create a copy of the [What’s New project](https://github.com/adamichelle/whats-new) by clicking on the **Fork** button at the top right-hand corner of the GitHub web interface.

Then open your command line app and run the following command:

```
git clone https://github.com/{your-github-username}/whats-new.git
```

You have to replace the part of the URL highlighted in the command above with your actual GitHub username.

Navigate to the \`whats-new\`\` directory on your command line:

```
cd whats-new
```

Checkout the starter code branch `do/starter-code`:

```
git checkout do/starter-code
```

If you have not registered and retrieved your API key yet, you should do so now.

Next, using your favourite code editor, create a copy of the `.env.example` file and rename it to `.env`.

Replace the placeholder `your-news-api-key` value for `VITE_NEWS_API_KEY` with your API key from [News API](https://newsapi.org/) and save the `.env` file.

Install the project dependencies:

```
npm install
```

Run the application:

```
npm run dev
```

You should see the following output in the terminal:

```
> whats-new@0.0.0 dev
> vite

  VITE v5.0.10  ready in 3639 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

When you launch the app in your browser, you will see the following screen:

![what's new homepage](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/3yw362u.png)

What’s New Homepage with news items

[Step 2 - Creating a web app manifest configuration](#step-2-creating-a-web-app-manifest-configuration)[](#step-2-creating-a-web-app-manifest-configuration)
------------------------------------------------------------------------------------------------------------------------------------------------------------

A web application manifest is a JSON-based file that provides a centralized location for storing the metadata associated with a web application.

In progressive web app development, a web manifest is used to provide the information that the browser needs to install the web application on a device such as the app name and icon.

At its core, a web app manifest for a progressive web app should include four top-level keys also known as members for it to be valid. They are: `name`, `icons`, `start_url` and `display`.

You can manually write the configuration for your web app manifest file and generate the icons needed. However, for the sake of this tutorial, you will be using this [PWA Manifest Generator](https://www.simicart.com/manifest-generator.html/) tool to generate both the configuration and the icons.

Launch the tool and fill out the form as shown below:

![PWA Manifest Generator Screen](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/elUkKXx.png)

Generating the configuration for a manifest using the PWA Manifest Generator

For the **icons** field, upload the `app-icon-image.png` file found at the root of the `whats-new` project folder.

Click on the **Generate Manifest** button to generate the manifest. The tool will generate a zipped folder that you can download to your machine. Extract the files from the zipped folder.

You should see a `manifest.webmanifest` file and some image files prefixed with the side of each icon. Copy the image files to your application’s `/public` folder.

Open the `manifest.webmanifest` file in any text editor. Its contents should be similar to the snippet shown in the image below.

![Manifest file screenshot](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/G1PO9lw.png)

Screenshot the manifest files generated.

You will need to use the JSON object in the file in the next section.

[Step 3 - Generating the web app manifest and service worker](#step-3-generating-the-web-app-manifest-and-service-worker)[](#step-3-generating-the-web-app-manifest-and-service-worker)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

In this step, you will generate the web application manifest for the application with the configuration above and create a service worker using a Vite plugin called [`vite-plugin-pwa`](https://vite-pwa-org.netlify.app/).

This customizable plugin allows you to add PWA capabilities to an existing Vite application.

To add the plugin to your application, you need to install it via `npm`:

```
npm install -D vite-plugin-pwa
```

Next, you configure the plugin for your application by editing your `vite.config.js` file as shown below:

vite.config.js

```
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { VitePWA } from 'vite-plugin-pwa'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    VitePWA({})
  ],
})
```

The `vite-plugin-pwa` package exports a function called `VitePWA` that receives an object with a set of different options for configuring the plugin as an argument. One of the options you can set for this plugin is the `manifest` option. By setting this option, you are giving the plugin the manifest configuration you want the plugin to use to generate the web app manifest.

Add a `manifest` key to the object in the `VitePWA()` function call, set it to the JSON object from the generated `manifest.webmanifest` file and save your changes. You should end up with a Vite configuration object that looks like this:

vite.config.js

```
export default defineConfig({
  plugins: [
    vue(),
    VitePWA({
      manifest: {
        "theme_color": "#0F172A",
        "background_color": "#f5f8fa",
        "display": "standalone",
        "scope": "/",
        "start_url": "/",
        "name": "What's New - Vue News Aggregator Site",
        "short_name": "What's New",
        "description": "A news aggregator pulling news items from News API.",
        "icons": [
          {
            "src": "/icon-192x192.png",
            "sizes": "192x192",
            "type": "image/png"
          },
          {
            "src": "/icon-256x256.png",
            "sizes": "256x256",
            "type": "image/png"
          },
          {
            "src": "/icon-384x384.png",
            "sizes": "384x384",
            "type": "image/png"
          },
          {
            "src": "/icon-512x512.png",
            "sizes": "512x512",
            "type": "image/png"
          }
        ]
      }
    })
  ],
})
```

You should now have a working installable PWA. Every time you build your app, the plugin generates a web app manifest for your browser and configures it on your application’s entry point which is /. It also creates a service worker and it adds the script to register the service worker in your browser. How can you verify the presence of these artefacts in your browser to ensure that your app is now a PWA?

### [Verifying the PWA](#verifying-the-pwa)[](#verifying-the-pwa)

You need to open your browser developer tools and navigate to the **Application** tab. In the course of this tutorial, we will be using the Chrome browser as our reference. To open your developer tools in Chrome, press the keys `CTRL+SHIFT+I` (windows) or `OPTION+CMD+I` (MAC). Under the **Application** tab in the sidebar, you should see a section called **Application**. That section has a sub-link called **Manifest**. When you click on that link, you should see some information correlating with what you have provided to the plugin above. However, at this stage, you are presented with the screen below.

![Picture of the empty manifest section in the Application tab in the browser dev tools](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/8jTBrze.png)

Empty **Manifest** section in **Application** tab

Additionally, you will find that there are no service workers registered under the **Service workers** section.

Also, under the **Storage** section, when you click on **Cache storage**, you should find some cached information but again as with the **Manifest**\* section, you will find that it is empty.

You should also find a desktop with an arrow-down icon button in the browser toolbar section.

![Browser Toolbar with a desktop with arrow down icon button for installing a PWA.](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/Ec1YGA9.png)

Toolbar with install icon button

These screens are currently blank and you cannot see the button because you are currently viewing your application in the browser while running it in dev mode. This behaviour is expected because you’re running your application in dev mode and the plugin is configured to generate those artefacts and produce the expected behavior in production mode by default. In the next two sections, you will learn about the options you have to address this issue.

### [Building and previewing the app](#building-and-previewing-the-app)[](#building-and-previewing-the-app)

You have the option to preview the app by running the `npm run build` and `npm run preview` commands consecutively to verify your PWA and launch the URL in the browser.

The build preview URL is different from the URL generated by the dev server for dev builds.

Repeat the process described above. You should now see the manifest, some items in the cache storage and the install button (marked in yellow) as shown in the images below:

![Manifest section showing the information specified in the manifest configuration and icons uploaded to the public folder](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/LvINxKM.png)

**Manifest** section with configuration data

![Cache storage section with newly added cache storage.](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/il7oggd.png)

**Cache storage** section

This option requires rebuilding the app anytime you make any changes to your code and can be quite an inconvenience. The next option eliminates this inconvenience as you will see below. However, it comes with some caveats that you’ll discover in the next main step. However, at this point in the tutorial, for our current purpose, you will learn how to work with the plugin in dev mode.

### [Using the `devOptions` option in the Vite PWA plugin configuration](#using-the-devoptions-option-in-the-vite-pwa-plugin-configuration)[](#using-the-devoptions-option-in-the-vite-pwa-plugin-configuration)

The `vite-plugin-pwa` package provides a `devOptions` option to support the ability to view the app as a PWA in dev mode from version `v0.11.13`. You set the `enabled` sub-option to `true` as shown in the lines highlighted in the code block below:

vite.config.js

```
export default defineConfig({
  plugins: [
    vue(),
    VitePWA({
      devOptions: {
        enabled: true
      },
      manifest: {...}
    })
  ],
})
```

When you save the changes and restart the dev server, you will notice some additional lines added to your console output as shown in the highlighted lines below.

```
Output> whats-new@0.0.0 dev
> vite

  VITE v5.0.10  ready in 1490 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help

PWA v0.17.4
mode      generateSW
precache  2 entries (0.12 KiB)
files generated
  dev-dist\sw.js
  dev-dist\workbox-9637eeee.js
12:28:22 p.m. [vite] vite.config.js changed, restarting server...
12:28:22 p.m. [vite] server restarted.
```

The logs tell you what mode the plugin is configured to use. It is configured to generate a service worker, hence the `generateSW` value you see. The logs also show that there are some files generated in the `dev-dist` folder. The `sw.js` file is the service worker script that is created by the plugin. This script contains the caching strategy. Because the plugin leverages the `workbox-build` package, it also generates an id-associated workbox file that is used to build the service worker. Lastly, it also generates a file called `registerSW.js` which is not shown in the logs. The `registerSW.js` file is the script that checks if the browser supports service workers and registers the service worker when the application is loaded.

When the application is launched and loaded in your browser, the `registerSW.js` file registers the service worker created in `sw.js` and you can see the updates made to our browser. Retry the steps above to verify the **Manifest** and **Cache storage** interfaces in your browser.

Additionally, you can see the active Service Worker in the **Service workers** section of the **Application** tab as shown below:

![Registered and active service worker working in the background for the current application tab showing the dev server code.](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/NTnvf5M.png)

Active service worker

[Step 4 - Handling the caching of application files and assets](#step-4-handling-the-caching-of-application-files-and-assets)[](#step-4-handling-the-caching-of-application-files-and-assets)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

At this point of the tutorial, your application is a PWA but it is not offline-ready quite yet. If you want to verify this, navigate to the **Network** tab of your browser dev tools and set it to run offline. You’ll find that you are presented with a blank screen.

You need to cache your application files and assets to support offline behaviour. To do that you need to make additional modifications to your configuration for the Vite PWA plugin as shown in the highlighted lines below:

vite.config.js

```
export default defineConfig({
  plugins: [
    vue(),
    VitePWA({
      devOptions: {...},
      includeAssets: [
        "**/*"
      ],
      manifest: {...},
      workbox: {
        globPatterns: ["**/*.{js,css,html,png}"]
      }
    })
  ],
})
```

The `includeAssets` option tells the plugin to include the other static assets found in the `public` folder in the service worker precache. Examples of such static assets are your website favicon, SVG files and font files you may have added to your `public` folder.

The icons added to the `public` directory earlier are included by default since they were added under the `manifest` option in the plugin configuration.

The `globPatterns` option under `workbox` is used to restrict the type of application files you want to cache. In this case, you want to cache the `.js`, `.css`, `.html`, and `.png` files that are your main application files.

With these new changes, your application should now be able to run in offline mode. To see it in action, you have to open another terminal and rebuild the application using `npm run build` if you’re currently running it in dev mode.

You might ask: ‘Why is it different for this situation?’ ‘Doesn’t the `devOptions` option suffice to handle all things related to making a progressive web application in dev mode?’ The answers to these questions all boil down to revising the difference between the builds generated when running the dev server and production builds.

When running the application in dev mode in Vite, the development build is managed in memory and not written to disk. In other words, there is no build output of the application files in a `dist` folder like you would get with a production application build. For that reason, there’s nothing for the service worker to add to the precache. If there is nothing to add to the precache, then nothing would be cached in the cache storage in the browser. The only items added to the precache by default are the `index.html` file i.e. the entry point and the `registerSW.js` file.

The `index.html` file requires the `main.js` file to load everything needed for your application as you may already know and since this file or any of the other files it imports are not cached, you will always be presented with a blank screen when you run your application in dev mode as an offline application.

To verify that they are not cached, after running a production build of your application, compare the contents of the `sw.js` files in both the `dev-dist` and `dist` directories. You should be looking out for the items provided as arguments to a `precacheAndRoute` function.

Run the production build of your application using `npm run preview` and launch it on the browser. You can place it side-by-side with the browser tab running your dev build. Navigate to the **Network** tab of your browser dev tools, set it to run offline and refresh the page. You should still be able to see the application screen.

This is because when your application was loaded, the service worker was registered and the build files were cached. To see the files, navigate to the **Cache storage** section in the **Application** tab and click on the cached item labeled **workbox-precache-\***. You should see file names corresponding to the files in your application’s `dist` folder.

Here is a screenshot for your reference:

![Image showing the domain production build of the application, the number of cache entries and the list of cached build files in the Cache storage](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/of4zTtW.png)

**Cache** storage with dist files

You can also compare it with the cache for your dev server browser tab. Here is another screenshot for your reference:

![Image showing the domain of dev run of the application, the number of cache entries and the list of cached build files in the Cache storage](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/gbZSQnh.png)

**Cache** storage without dist files

Some key things to note are the difference in the number of entries (marked in blue), the port numbers (marked in yellow) and the table of files cached (marked in red).

If you made it to this point, congratulations! You have thus been able to make your application offline-available successfully. What’s the next step? Your application displays some data fetched from an API. Right now, only the application files are cached. In the next section, you will add indexDB to fix this gap.

[Step 5 - Caching application data using IndexDB](#step-5-caching-application-data-using-indexdb)[](#step-5-caching-application-data-using-indexdb)
---------------------------------------------------------------------------------------------------------------------------------------------------

IndexedDB is a browser API for storing structured data and blobs (binary data) on the client side. It is a transactional database system that stores data as key-value pairs in a JavaScript object-like structure.

In this section, you will add an IndexedDB database to your application and use it to manage data caching in your application.

### [Update the app to retrieve data from API](#update-the-app-to-retrieve-data-from-api)[](#update-the-app-to-retrieve-data-from-api)

To begin, you need to make some changes to the way the data shown on the app is retrieved. Currently, the data being displayed is stored as in-app variables.

Open the `NewsItems.vue` file. Delete the `testNewsItemsData` variable and the associated comment.

Locate the part of the function called `getCustomizedTabNewsItems` within the `<script setup>` tag shown below and do what is said within the comments:

NewsItems.vue

```
const getCustomizedTabNewsItems = () => {
  //...
  if (definedCustomizations) {
    //...
    /** TODO: Remove the the line below after setting up your API KEY and delete this comment */
    newsItems.value = [
      {
        source: {
          id: 'buzzfeed',
          name: 'Buzzfeed'
        },
        author: 'Melanie Aman',
        title: '37 Beauty Products Under $25 That Don\'t Skimp On Results',
        description: 'A heavy serving of getting more than you pay for ...don\'t mind if we do.View Entire Post ›',
        url: 'https://www.buzzfeed.com/melanie_aman/beauty-products-under-25-that-dont-skimp-on-results',
        urlToImage: 'https://img.buzzfeed.com/buzzfeed-static/static/2023-12/11/20/enhanced/46ff3dce9b4d/original-451-1702327901-3.jpg?crop=4205:2208;0,0%26downsize=1250:*',
        publishedAt: '2023-12-24T09:00:03Z',
        content: 'Psst! Bio-Oil contains retinol, which accelerates skin turnover but can make you more sensitive to the sun so don\'t forget your sunscreen!\r\nPromising review: \'I was skeptical of how amazing the revie… [+689 chars]'
      },
      // ...
    ]

    /** TODO: Uncomment after setting up your API KEY */
    // const { fetchedNewsItems, getNewsItems } = useNewsItems(requestUrl)
    // nextTick(async () => {
    //   await getNewsItems()
    //   newsItems.value = fetchedNewsItems.value
    // })
  }
}
```

Go a little further down the file still within the `<script setup>` tag and locate the code highlighted below:

NewsItems.vue

```
if (props.tab.id === APPLICATION_TABS[2].id && props.retrieveCustomCuratedContent) {
  // ...
} else if (props.tab.id === APPLICATION_TABS[2].id && !props.retrieveCustomCuratedContent) {
  // ...
} else {
  // get news items for other tabs ...
  // TODO: Remove the the line below after setting up your API KEY and delete this comment
  newsItems.value = testNewsItemsData

  /** TODO: Uncomment after setting up your API KEY */
  // const { fetchedNewsItems, getNewsItems } = useNewsItems(requestUrl)
  // nextTick(async () => {
  //   await getNewsItems()
  //   newsItems.value = fetchedNewsItems.value
  // })
}
```

Also, do what the comment says.

Finally, open the `SourceToggleTokens.vue` file and find the code shown below:

SourceToggleTokens.vue

```
watch(sourcesRetrievalParameters, async (newParams) => {
  // ...

  /** TODO: Uncomment after setting up your API KEY */
  // const { newsSources: apiNewsSources, getNewsSources } = useNewsSources(queryString.value)
  // await getNewsSources()
  // newsSources.value = apiNewsSources.value
})
```

Uncomment the code as the instructions tell you to and save your changes.

If you have added your API key as expected in the beginning and updated the files as discussed above, you should be presented with a different set of news items on the home page.

![Headline tab with a list of headlines gotten from the API](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/sibRLVg.png)

News Items from API

Additionally, when you click on the setting icon highlighted in blue above, you will be presented with a long list of news sources similar to the screen shown below:

![Screen showing the setting section where the list of news sources from the API is rendered](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/HP40T1H.png)

News Sources from the API

### [Determining what to store](#determining-what-to-store)[](#determining-what-to-store)

Until now, you’ve only focused on the visuals of the application. To understand what to store, we’ll go through a brief walkthrough of how the application queries the News API.

The application has 3 tabs: **Headlines**, **Explore** and **Curated for me**. The file called `constants/general.js` holds information for each of those tabs in a variable called `APPLICATION_TABS`. Each tab object has two properties called `apiCallURL` and `apiCallParameters`. You’ll notice that each `apiCallURL` value is a string that starts with a given endpoint for querying the News API and has some placeholder values in curly braces representing the expected request parameters for that endpoint. Those placeholder values are the same as the elements in the corresponding `apiCallParameters` array for that tab. Take a look at the object for the **Headlines** tab shown below:

constants/general.js

```
export const APPLICATION_TABS = [
  {
    name: 'Headlines',
    id: 'headlines-tab',
    target: '#headlines',
    ariaControls: 'headlines',
    apiCallURL: 'https://newsapi.org/v2/top-headlines?country={country}&category={category}',
    apiCallParameters: ['category', 'country']
  },
  { // ... },
  { //... }
]
```

The URL gets formatted to produce the complete endpoint using a function called `parse` from the `dynamic-string-parser.js` util. Then it gets passed as an argument to a function called `useNewsItems`. The `useNewsItems` function is a composable that returns two things: `fetchedNewsItems` - a reactive array that holds the data from the API and `getNewsItems` - a function that does the actual network fetch and updates the `fetchedNewsItems` object.

We call the `getNewsItems` function and set the returned array to `newsItems`.

The snippet below shows the `getNewsItems` function:

useNewsItems.js

```
async function getNewsItems() {
  try {
   const apiResponse = await fetch(url.value, {
    headers: {
     'X-Api-Key': import.meta.env.VITE_NEWS_API_KEY
    }
   })
   const data = await apiResponse.json()
   fetchedNewsItems.value = data.articles
   count.value = data.totalResults
  } catch (error) {

  }
 }
```

You will update this function to store the news items from the API in an indexedDB database that you will create in the next section.

### [Installing and setting up an IndexedDB database in the app](#installing-and-setting-up-an-indexeddb-database-in-the-app)[](#installing-and-setting-up-an-indexeddb-database-in-the-app)

First, you need to install the [`idb`](https://www.npmjs.com/package/idb) package from `npm`:

```
npm install idb
```

The `idb` package is a lightweight wrapper for the IndexedDB API used to interact with this client-side storage. The IndexedDB API has a reputation for being a little quirky to use in its raw form and not user-friendly. This package provides enhancements that make it easier to use.

Next, you need to create a composable file called `useIDB` and add the following code to it. This composable with contain reusable code that we can plug into our components that fetch data from the News API.

useIDB.js

```
import { openDB } from 'idb'
import { ref } from 'vue'

const versionNumber = ref(1)

const useIDB = () => {
    const db = ref(null)

  return {
   db,
   versionNumber
  }
}

export default useIDB
```

This composable returns the variable `db` which will you instantiate soon using the `openDB` function from `idb` as well as a global `versionNumber.` The reason why the `versionNumber` is at the top of the file is to ensure that it is global to the app at any time during its execution and we do not create a new one each time we call the composable.

The snippet below builds on the above and assigns the returned value to `db` after opening a database using `openDB`.

```
const useIDB = () => {
    const db = ref(null)
    const getDB = async (version, objectStoreName, keyPath) => {
        versionNumber.value += 1
        db.value = await openDB('whats-new', version, {
            upgrade(db, oldVersion) {
                if (version === 1 && oldVersion === 0) {
                    db.createObjectStore(objectStoreName, {
                        keyPath
                    })
                }

                if (version > 1) {
                    if (!db.objectStoreNames.contains(objectStoreName)) {
                        db.createObjectStore(objectStoreName, {
                            keyPath
                        })
                    }
                }
            }
        })
    }

    return {
        db,
        versionNumber,
        getDB
    }
}
```

The new function called `getDB` is an async function that takes in 3 arguments `version`, `objectStoreName` and `keyPath`. There are several terms used when talking about the IndexedDB API. One is the version of the database. This comes in handy when you need to modify the database in some way. Another is the concept of _object stores_.

Think of object stores as similar to tables in an SQL database.

Finally, we have something known as a _keyPath_. The _keyPath_ is a unique field or column name in the data you want to add to an object store.

The `getDB` function requires these three parameters from the consuming client component to create the database as seen in the code above. After incrementing the `versionNumber`, the value of `db` is set to the result of calling `openDB` with the name of the database we want to create - `'whats-new'` - and the version number.

The `openDB` function also has an object as the third argument. This object contains events used to set up the database.

In this use case, you only need the `upgradeDb` event. The `upgradeDb` event runs when the version number of the database created is greater than that of the existing database. If `oldVersion` - the current version number of the database existing in the browser - is `0` and the version we want to create is `1`, you want to create an object store with the `objectStoreName` and `keyPath` provided.

However, if the version we want to create is greater than 1, you create an object store with the `objectStoreName` and `keyPath` provided only if there’s no object store with the given `objectStoreName`.

Finally, you return the `getDB` function.

The last thing this composable needs to do is provide us with an interface to retrieve data from the `db` if present. See the code snippet below for how to achieve that:

useIDB.js

```
const useIDB = () => {
    const db = ref(null)
    // getDB

    const getDataFromObjectStore = async (objectStoreName) => {
        let data

        if (db.value) {
            data = await db.value.getAll(objectStoreName)
        } else {
            const db = await openDB('whats-new', undefined)
            data = await db.getAll(objectStoreName)
        }

        return data
    }

    return {
        //...
        getDataFromObjectStore
    }
}
```

If `db.value` is not `null`, retrieve all data from the object store. Otherwise, open the current `db` in the browser and retrieve all data.

With the composable created, you can make the needed changes to `getNewsItems` as shown below:

useNewsItems.js

```
async function getNewsItems() {
  const { db, getDB, versionNumber, getDataFromObjectStore } = useIDB()

  try {
   // ... apiResponse
   const data = await apiResponse.json()
   // setting fetchedNewsItems

   await getDB(versionNumber.value, url.value, 'url')
   data.articles.forEach(async (article) => {
    await db.value.put(url.value, article)
   })
  } catch (error) {
   if (error instanceof TypeError && error.message.includes('Failed to fetch')) {
    const cachedItems = await getDataFromObjectStore(url.value)
    fetchedNewsItems.value = cachedItems
   }
  }
 }
```

If the attempt to fetch the articles from the API is successful, you call `getDB` and store the articles in the database using the `put` function from the indexedDB API. The `put` function adds the item to the object store specified as the first parameter.

If the fetch attempt fails, you check that the error is a fetch error and retrieve the data from the database using `getDataFromObjectStore`.

You can do likewise for `getNewsSources` in `useNewsSources` as shown below:

useNewsSources.js

```
async function getNewsSources() {
  const { db, getDB, versionNumber, getDataFromObjectStore } = useIDB()
    try {
      // apiResponse
      const data = await apiResponse.json()
      // ...
      await getDB(versionNumber.value, baseUrl.value, 'id')
 data.sources.forEach(async (source) => {
  await db.value.put(baseUrl.value, source)
 })
    } catch (error) {
  if (error instanceof TypeError && error.message.includes('Failed to fetch')) {
   const cachedItems = await getDataFromObjectStore(baseUrl.value)
   newsSources.value = cachedItems
  }
    }
  }
```

Save your changes, rebuild your app and run it in preview mode using the commands below:

```
npm run build
```

```
npm run preview
```

Launch the app in your browser. You should see a new entry under the **IndexedDB** sub-section of the **Storage** section in the **Application** tab of your browser dev tools.

![New entry called whats-new under the IndexedDB section in the Application tab of the browser dev tools with two stores corresponding to the URLs fetched](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/sh7kvA1.png)

New IndexedDB entry

Next, navigate to the Network tab. Set the network throttling option to Offline as shown below. You should see a small yellow warning icon.

![Image of the Network tab with the throttling setting set to offline](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/W4DO-2025/Building-news-aggregator-app/jEGKLEB.png)

Network tab set to offline

Reload the app. Your application should load as before.

There might be a temporary moment where you get the `No internet` screen but the app will boot up immediately after.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In this article, you converted a single-page application in Vue to a progressive web app using service workers and the IndexedDB API. Possible next steps include alerting users about new updates to the page and prompting them to reload the page to see the updated version. The [Vue section](https://vite-pwa-org.netlify.app/frameworks/vue.html) in the documentation for the `vite-plugin-pwa` package provides a good starting point.

#### [Source](https://www.digitalocean.com/community/tutorials/building-news-aggregator-web-app-using-vite-vue)

<br/>
---
