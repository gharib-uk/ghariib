---
title: "Optimizing Performance with Infinite Scrolling in React"
date: 2025-02-06
---

In our daily lives, we interact with many applications like Instagram, YouTube, LinkedIn, Facebook etc. If we observe carefully, the content loads continuously as we scroll. This seamless experience is achieved using infinite scrolling, a technique that keeps users engaged without interruptions.

## Why Infinite Scrolling?

Loading or processing a huge amount of data all at once during page load is not a good idea. It can negatively impact:

**Performance** – Fetching large amounts of data at once can slow down the application.  
**User Experience** – Users might face delays or lag while interacting with the application.

To avoid these issues, applications implement infinite scrolling. This approach enhances engagement and provides a smooth user experience by dynamically loading content as the user scrolls.

## Implementing Infinite Scrolling in React

There are multiple ways to implement infinite scrolling in React. One of it is by using the `react-infinite-scroll-component` library.

This library helps manage infinite scrolling efficiently by handling API calls and updating the UI without affecting performance.

**1\. Define State variables**  

```
const [items, setItems] = useState([]);
const [hasMore, setHasMore] = useState(true);
```

`items:` Stores the list of items displayed on the page.  
`hasMore:` A boolean that determines whether more data is available to load.

**2\. Function to Generate New Data**  
This function generates an array of 10 numbers, starting from a given number.  

```
const generateArray = (startFrom) => {
  return Array.from({ length: 10 }, (_, index) => startFrom + index);
};
```

**3\. Fetching Initial Data on Component Mount**  
When the component mounts, it triggers `getInitialData()`, which simulates a delayed API call using `setTimeout()`.  

```
useEffect(() => {
  async function getInitialData() {
    setTimeout(() => {
      setItems(generateArray(1));
    }, 1000);
  }
  getInitialData();
}, []);
```

**4\. Handling Data Fetching on Scroll**  

```
const getData = async () => {
  if (items.length < 60) {
    console.log("Fetching the data after", items.length);
    setTimeout(() => {
      setItems((prevItems) => [
        ...prevItems,
        ...generateArray(prevItems.length + 1),
      ]);
      setHasMore(true);
    }, 1000);
  } else {
    console.log("-----------No More Data to show----------");
    setHasMore(false);
  }
};
```

- This `getData()` function loads more data when the user scrolls down.
- If `items.length` is less than 60, it fetches the next batch of 10 items.
- If the length reaches 60, it stops loading more data by setting `hasMore`to `false`.

**5\. InfiniteScroll Component**  

```
<InfiniteScroll
  dataLength={items.length}
  next={getData}
  hasMore={hasMore}
  loader={<p>Loading....</p>}
>
  {items.map((value, index) => (
    <div
      key={value + index}
      style={{
        height: "100px",
        fontSize: "20px",
        fontWeight: "bold",
        backgroundColor: `${index % 2 === 0 ? "" : "lightblue"}`,
        marginBottom: "10px",
      }}
    >
      {value}
    </div>
  ))}
</InfiniteScroll>
```

## Full Code

```
import { useEffect, useState } from "react";
import InfiniteScroll from "react-infinite-scroll-component";

function InfiniteScrollDemo() {
  const [items, setItems] = useState([]);
  const [hasMore, setHasMore] = useState(true);

  const generateArray = (startFrom) => {
    return Array.from({ length: 10 }, (_, index) => startFrom + index);
  };

  useEffect(() => {
    async function getInitialData() {
      setTimeout(() => {
        setItems(generateArray(1));
      }, 1000);
    }
    getInitialData();
  }, []);

  const getData = async () => {
    if (items.length < 60) {
      console.log("Fetching the data after", items.length);
      setTimeout(() => {
        setItems((prevItems) => [
          ...prevItems,
          ...generateArray(prevItems.length + 1),
        ]);
        setHasMore(true);
      }, 1000);
    } else {
      console.log("-----------No More Data to show----------");
      setHasMore(false);
    }
  };

  return (
    <div className="App">
      <InfiniteScroll
        dataLength={items.length}
        next={getData}
        hasMore={hasMore}
        loader={<p>Loading....</p>}
      >
        {items.map((value, index) => (
          <div
            key={value + index}
            style={{
              height: "100px",
              fontSize: "20px",
              fontWeight: "bold",
              backgroundColor: `${index % 2 === 0 ? "" : "lightblue"}`,
              marginBottom: "10px",
            }}
          >
            {value}
          </div>
        ))}
      </InfiniteScroll>
    </div>
  );
}

export default InfiniteScrollDemo;
```

## Key Takeaways

This implementation efficiently loads content in batches, ensuring that the app does not load excessive data at once. The benefits include:  
**Better performance** – Only necessary data is loaded.  
**Smooth user experience** – Users don’t have to wait for large data loads.  
**Engagement-friendly** – Continuous scrolling keeps users active on the page.

Go to Source
