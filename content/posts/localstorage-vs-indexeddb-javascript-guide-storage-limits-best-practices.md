---
title: "<div>LocalStorage vs IndexedDB: JavaScript Guide (Storage, Limits & Best Practices)</div>"
date: 2025-03-19
---

When building web applications, efficient data storage is essential. JavaScript provides two key client-side storage options: **LocalStorage** and **IndexedDB**. This blog will cover their differences, usage, limits, and best practices.

## 1\. What is LocalStorage?

**LocalStorage** is a simple key-value storage API that allows storing small amounts of data in the browser.

### Features:

- Stores data as **strings**.
- **Synchronous** (may block UI).
- Limited to **~5MB per origin**.
- **Persistent storage** (data remains after page refresh or browser restart).
- No **indexing or query support**.

### Use Cases:

- Caching small data (e.g., user preferences, themes).
- Storing authentication tokens (consider security risks).
- Saving basic app settings.

### How to Use LocalStorage

#### **Set Data**

```
localStorage.setItem("username", "JohnDoe");
```

#### **Get Data**

```
const user = localStorage.getItem("username");
console.log(user); // "JohnDoe"
```

#### **Remove Data**

```
localStorage.removeItem("username");
```

#### **Clear All Data**

```
localStorage.clear();
```

## 2\. What is IndexedDB?

**IndexedDB** is a low-level, asynchronous, NoSQL database for storing large amounts of structured data.

### Features:

- Stores **objects** (not just strings).
- **Asynchronous** (non-blocking operations).
- **Much larger storage** capacity (can use up to 50% of available disk space).
- Supports **transactions and indexing** for faster queries.
- **Persistent storage** (data is not cleared unless deleted by user/browser policy).

### Use Cases:

- Storing large datasets (e.g., product catalogs, offline data sync).
- Storing images, files, or blobs.
- Caching API responses efficiently.

### How to Use IndexedDB

#### **Open or Create a Database**

```
const request = indexedDB.open("MyDatabase", 1);

request.onupgradeneeded = function(event) {
  const db = event.target.result;
  if (!db.objectStoreNames.contains("users")) {
    db.createObjectStore("users", { keyPath: "id" });
  }
};
```

#### **Add Data**

```
request.onsuccess = function(event) {
  const db = event.target.result;
  const txn = db.transaction("users", "readwrite");
  const store = txn.objectStore("users");

  store.add({ id: 1, name: "Alice", age: 25 });

  txn.oncomplete = () => console.log("Data added!");
};
```

#### **Read Data**

```
request.onsuccess = function(event) {
  const db = event.target.result;
  const txn = db.transaction("users", "readonly");
  const store = txn.objectStore("users");

  const getRequest = store.get(1);
  getRequest.onsuccess = () => console.log(getRequest.result);
};
```

#### **Delete Data**

```
request.onsuccess = function(event) {
  const db = event.target.result;
  const txn = db.transaction("users", "readwrite");
  const store = txn.objectStore("users");

  store.delete(1);
  txn.oncomplete = () => console.log("Data deleted!");
};
```

## 3\. LocalStorage vs IndexedDB: Key Differences

| Feature | LocalStorage | IndexedDB |
| --- | --- | --- |
| Data Type | Strings | Objects (JSON, files, blobs) |
| Capacity | ~5MB | Up to 50% of disk space |
| Performance | Synchronous (blocks UI) | Asynchronous (non-blocking) |
| Query Support | No | Yes (Indexed queries) |
| Transactions | No | Yes (ACID transactions) |
| Security | Exposed to JavaScript (XSS risk) | More secure (sandboxed) |

## 4\. Storage Limits & Best Practices

### **Storage Limits**

- **LocalStorage**: Limited to ~5MB per origin.
- **IndexedDB**: Can use **gigabytes** of space, but browsers may prompt users when reaching high storage.
- **Safari Warning**: Safari may auto-delete IndexedDB data if storage is low.

### **Best Practices**

- **Use LocalStorage for small, simple key-value storage.**
- **Use IndexedDB for complex data, large files, or offline functionality.**
- **Compress data** (e.g., `JSON.stringify` + gzip for LocalStorage).
- **Handle errors** (e.g., `QuotaExceededError` in LocalStorage).
- **Use `navigator.storage.estimate()`** to check available space.

```
navigator.storage.estimate().then(({ quota, usage }) => {
  console.log(`Used: ${usage} bytes`);
  console.log(`Total available: ${quota} bytes`);
});
```

## 5\. Conclusion: Which One Should You Use?

| Use Case | Recommended Storage |
| --- | --- |
| Small settings, themes | LocalStorage |
| User authentication tokens | LocalStorage (short-term, with security considerations) |
| Large datasets (e.g., e-commerce products) | IndexedDB |
| Offline caching | IndexedDB |
| Images, files, blobs | IndexedDB |
| Fast, indexed queries | IndexedDB |

For most **modern web apps**, IndexedDB is the **better choice** due to scalability, performance, and security. However, LocalStorage is useful for **quick and simple storage needs**.

Go to Source
