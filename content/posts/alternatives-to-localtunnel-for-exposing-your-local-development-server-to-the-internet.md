---
title: "Alternatives to LocalTunnel for exposing your local development server to the internet"
date: 2025-01-03
categories: 
  - "general"
---

Here are some alternatives to **LocalTunnel** for exposing your local development server to the internet:

### 1\. **Ngrok**

- **Description**: One of the most popular tools for exposing local servers.
- **Features**:
    
    - Secure tunnels with HTTPS.
    - Detailed request logging and replay.
    - Supports custom subdomains (paid plans).
    

- **Command**:  
    
    ```
     ngrok http 8080
    ```
    

- **Website**: https://ngrok.com
    

### 2\. **Cloudflare Tunnel (Argo Tunnel)**

- **Description**: A tool by Cloudflare to securely expose local servers to the web.
- **Features**:
    
    - Free for basic use.
    - Built-in DDoS protection.
    - Easy integration with Cloudflare services.
    

- **Command**:  
    
    ```
     cloudflared tunnel --url http://localhost:8080
    ```
    

- **Website**: https://developers.cloudflare.com/cloudflare-one/connections/connect-apps
    

### 3\. **Serveo**

- **Description**: A simple and lightweight alternative.
- **Features**:
    
    - No installation required.
    - Custom subdomains available.
    

- **Command**:  
    
    ```
     ssh -R 80:localhost:8080 serveo.net
    ```
    

- **Website**: https://serveo.net
    

### 4\. **Expose**

- **Description**: A PHP-based alternative for exposing local servers.
- **Features**:
    
    - Works well with Laravel and other PHP frameworks.
    - Custom subdomains.
    

- **Command**:  
    
    ```
     expose share http://localhost:8080
    ```
    

- **Website**: https://expose.dev
    

### 5\. **Inlets**

- **Description**: A self-hosted tunnel solution.
- **Features**:
    
    - Ideal for developers who prefer self-hosted solutions.
    - Paid version for production use.
    

- **Command**:  
    
    ```
     inlets client --remote wss://your-server --upstream=http://localhost:8080
    ```
    

- **Website**: https://inlets.dev
    

### 6\. **Pagekite**

- **Description**: A reliable and long-standing option.
- **Features**:
    
    - Supports HTTP, HTTPS, and SSH tunneling.
    - Free for personal use with bandwidth limits.
    

- **Command**:  
    
    ```
     pagekite.py 8080 yourname.pagekite.me
    ```
    

- **Website**: https://pagekite.net
    

### Comparison

| Feature | Ngrok | Cloudflare Tunnel | Serveo | Expose | Inlets | Pagekite |
| --- | --- | --- | --- | --- | --- | --- |
| Free Tier | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Custom Subdomain | ✅ (Paid) | ✅ | ✅ | ✅ | ✅ | ✅ |
| Installation | Required | Required | Not Req. | Required | Required | Required |
| Self-Hosting | ❌ | ✅ | ❌ | ❌ | ✅ | ❌ |

Choose based on your specific requirements, such as ease of use, security, or budget!

Go to Source
