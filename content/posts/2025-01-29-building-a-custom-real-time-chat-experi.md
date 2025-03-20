---
title: "Building a Custom Real-time Chat Experience with DigitalOceans GenAI Platform"
date: 2025-01-29T18:26:40.367Z
draft: false
type: posts
categories: 
- development,deep-learning,ai-ml,droplets
---
# Building a Custom Real-time Chat Experience with DigitalOceans GenAI Platform

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

Imagine building a ChatGPT-like experience that you can fully customize to your needs. With DigitalOcean’s new GenAI Platform, which just launched into Public Preview at this year’s Deploy conference, you can do exactly that.

What’s particularly exciting about the GenAI Platform is its flexibility. For many use cases, you don’t need to write any code at all – you can simply grab a JavaScript snippet from the DigitalOcean Control Panel and embed a chatbot directly into your application. But as a developer, what really excites me is the ability to use the APIs to build any kind of experience I want.

In this post, I’ll walk through building a custom chat application that mimics the ChatGPT experience. This example will teach you three key components of building a robust chat application:

1.  API Authentication - Securely connecting to the GenAI Platform
2.  Real-time Communication with WebSockets - Setting up bi-directional communication
3.  Chat Service and Message Processing - Managing conversations and streaming responses

The best part is that once you understand all the pieces, you can customize it to work the way you want.

[Understanding the GenAI Platform](#understanding-the-genai-platform)[](#understanding-the-genai-platform)
----------------------------------------------------------------------------------------------------------

The [DigitalOcean GenAI Platform](/blog/introducing-generative-ai-platform) is an all-in-one solution for building and scaling AI agents quickly. It provides access to state-of-the-art Generative AI models from organizations like Anthropic, Meta, and Mistral AI, with a focus on making AI development accessible to everyone.

Here’s what makes it special:

-   **Instant Deployment**: Build and deploy AI agents in just a few clicks
-   **Foundation Models**: Access leading models from Anthropic, Meta, and Mistral AI
-   **Flexible Integration**: Choose between no-code chatbot embedding or full API access
-   **Built-in Security**: Customizable guardrails to filter harmful content
-   **Cost-Effective**: Transparent pricing for confident scaling
-   **Advanced Features**:
    -   Retrieval-augmented generation (RAG) for knowledge base integration
    -   Function routing for custom capabilities
    -   Agent customization and guardrails

The platform takes care of the infrastructure work, letting you focus on building your application regardless of your AI experience level.

[OpenAI API Compatibility](#openai-api-compatibility)[](#openai-api-compatibility)
----------------------------------------------------------------------------------

One of the most powerful aspects of the GenAI Platform is its API compatibility with OpenAI. This means you can use any OpenAI-compatible SDK or library to interact with the platform. If you’ve worked with OpenAI before, you already have all the experience you’ll need to use the GenAI Platform – and if you’re just getting started, you can leverage the extensive ecosystem of tools and libraries built for OpenAI.

Here’s what this compatibility means for developers:

-   Use your favorite OpenAI SDK (Python, Node.js, etc.)
-   Leverage existing code and examples
-   Access a rich ecosystem of tools and libraries
-   Minimal learning curve if you’re familiar with OpenAI
-   Easy migration path for existing applications

For example, in our chat application, we’ll use the OpenAI Node.js SDK:

```
const { OpenAI } = require('openai');

const client = new OpenAI({
    baseURL: agent_endpoint,
    apiKey: access_token,
});

const response = await client.chat.completions.create({
    model: "n/a", // Model is handled by the GenAI Platform
    messages: conversationHistory,
    stream: true,
});
```

This compatibility dramatically simplifies development and adoption. Instead of learning a new API or rewriting existing code, you can focus on building features that matter to your users.

[Building the Chat Application](#building-the-chat-application)[](#building-the-chat-application)
-------------------------------------------------------------------------------------------------

Before diving into the technical details, it’s worth noting that everything we’re about to explore is available in the [sample application on GitHub](https://github.com/wadewegner/do-genai-customchatbot). You can clone the repository, run it locally, and see these components in action. This makes it easier to follow along and experiment with the code yourself.

Let me dive into the three key components that make this chat application work.

### [API Authentication](#api-authentication)[](#api-authentication)

When working with APIs, secure authentication is essential. The GenAI Platform uses access keys to authenticate your requests, providing a simple and secure way to interact with the API without sending sensitive credentials with every call.

You’ll need to:

1.  Create an access key in the DigitalOcean Control Panel
2.  Store it securely in your application
3.  Use it to authenticate your API requests

Here’s how we can implement this in our chat application:

```
const { OpenAI } = require('openai');

class TokenService {
    constructor() {
        this.AGENT_ENDPOINT = process.env.AGENT_ENDPOINT + "/api/v1/";
        this.AGENT_KEY = process.env.AGENT_KEY;
        
        if (!this.AGENT_ENDPOINT || !this.AGENT_KEY) {
            throw new Error('Missing required configuration');
        }
    }

    getClient() {
        return new OpenAI({
            baseURL: this.AGENT_ENDPOINT,
            apiKey: this.AGENT_KEY,
        });
    }
}
```

This streamlined approach:

-   Uses a single access key for authentication
-   Requires minimal setup and maintenance
-   Follows security best practices
-   Works seamlessly with the OpenAI SDK

When making API calls to the GenAI Platform, we can simply use the client to interact with our agent:

```
const client = tokenService.getClient();
const response = await client.chat.completions.create({
    model: "n/a", // Model is handled by the GenAI Platform
    messages: conversationHistory,
    stream: true,
});
```

### [Real-time Communication with WebSockets](#real-time-communication-with-websockets)[](#real-time-communication-with-websockets)

WebSocket is a communication protocol that provides full-duplex communication channels over a single TCP connection. Unlike traditional HTTP requests, WebSockets maintain a persistent connection between the client and server, allowing for:

-   Real-time, bi-directional communication
-   Lower latency (no need for new handshakes after connection)
-   Efficient streaming of data
-   Better resource utilization

For this chat app, WebSockets are ideal because they allow us to:

1.  Stream AI responses in real-time as they’re generated
2.  Maintain connection state for each chat session
3.  Handle reconnection scenarios gracefully
4.  Provide immediate feedback on connection status

#### Server-Side Implementation

Here we’ve used the `ws` package, a popular and lightweight WebSocket client and server implementation for Node.js.

Here’s how we can set up the WebSocket server:

```
const { WebSocketServer } = require('ws');
const WS_PING_INTERVAL = 30000; // 30 seconds

// Create WebSocket server with no direct HTTP server attachment
const wss = new WebSocketServer({ noServer: true });

// Handle WebSocket connections
wss.on('connection', async (ws, req) => {
    // Extract chat session ID from URL parameters
    const chatId = new URL(req.url, 'ws://localhost').searchParams.get('chatId');
    if (!chatId) {
        ws.close(1008, 'Chat ID is required');
        return;
    }

    try {
        // Store connection in chat service
        chatService.addConnection(chatId, ws);
        
        // Implement connection health checks
        const pingInterval = setInterval(() => {
            if (ws.readyState === ws.OPEN) ws.ping();
        }, WS_PING_INTERVAL);
        
        // Clean up on connection close
        ws.on('close', () => {
            clearInterval(pingInterval);
            chatService.removeConnection(chatId);
        });
    } catch (error) {
        ws.close(1011, 'Failed to initialize connection');
    }
});

// Set up WebSocket upgrade handling
server.on('upgrade', (request, socket, head) => {
    wss.handleUpgrade(request, socket, head, (ws) => {
        wss.emit('connection', ws, request);
    });
});
```

Key aspects of the server implementation:

-   Handles WebSocket upgrades manually with `noServer: true`, giving us more control over the connection process and allowing us to validate session data before accepting connections
-   Implements ping/pong for connection health monitoring
-   Manages connections per chat session
-   Handles cleanup on connection close

#### Connection Management

Managing WebSocket connections properly is crucial for a reliable chat application. We need to track active connections, handle disconnections gracefully, and clean up resources when they’re no longer needed. Here’s how we can maintain active connections in the chat service:

```
class ChatService {
    constructor() {
        this.activeConnections = new Map();
        this.connectionTimeouts = new Map();
        this.CLEANUP_TIMEOUT = 5 * 60 * 1000; // 5 minutes
    }

    addConnection(id, ws) {
        // Remove any existing connection
        if (this.activeConnections.has(id)) {
            this.removeConnection(id);
        }

        // Store new connection
        this.activeConnections.set(id, ws);

        // Set up cleanup timeout
        ws.on('close', () => {
            this.connectionTimeouts.set(id, setTimeout(() => {
                this.conversations.delete(id);
                this.connectionTimeouts.delete(id);
            }, this.CLEANUP_TIMEOUT));
        });
    }

    removeConnection(id) {
        const connection = this.activeConnections.get(id);
        if (connection?.readyState === 1) {
            connection.send(JSON.stringify({ content: 'Connection closed' }));
        }
        this.activeConnections.delete(id);
    }
}
```

Let’s break down what’s happening here:

1.  **Connection Storage**:
    -   We use a `Map` to store active WebSocket connections, keyed by session ID
    -   Another `Map` tracks cleanup timeouts for disconnected sessions
    -   The 5-minute `CLEANUP_TIMEOUT` gives users time to reconnect without losing context
2.  **Adding Connections**:
    -   Before adding a new connection, we clean up any existing one for that session
    -   This prevents resource leaks and ensures one active connection per session
    -   Each connection is associated with a unique chat session ID
3.  **Cleanup Handling**:
    -   When a connection closes, we start a cleanup timer
    -   If the user doesn’t reconnect within 5 minutes, we remove their conversation history
    -   This balances keeping context for reconnecting users with freeing up resources
4.  **Connection Removal**:
    -   We notify the client before closing the connection if it’s still active
    -   All connection resources are properly cleaned up to prevent memory leaks

This careful management of connections helps ensure our chat application remains stable and efficient, even with many concurrent users.

#### Client-Side Implementation

The client-side WebSocket implementation needs to handle several important aspects of real-time communication:

-   Establishing and maintaining the connection
-   Handling disconnections gracefully
-   Processing incoming messages
-   Providing visual feedback to users

Here’s how we can implement these features:

```
const MAX_RECONNECT_ATTEMPTS = 5;
const RECONNECT_DELAY = 1000; // Start with 1 second
let reconnectAttempts = 0;
let ws = null;

function initializeWebSocket() {
    if (ws) {
        ws.close();
        ws = null;
    }

    // Determine WebSocket protocol based on page protocol
    const protocol = window.location.protocol === 'https:' ? 'wss:' : 'ws:';
    ws = new WebSocket(`${protocol}//${window.location.host}?chatId=${window.chatId}`);
    
    // Update UI connection status
    updateConnectionStatus('connecting', 'Connecting...');
    
    // Handle incoming messages
    ws.onmessage = (event) => {
        const data = JSON.parse(event.data);
        if (data.content === 'Connected to chat stream') {
            // Initial connection successful
            updateConnectionStatus('connected', 'Connected');
            reconnectAttempts = 0;
        } else if (data.content) {
            // Process chat message
            addMessage(data.content, false);
        } else if (data.error) {
            // Handle error messages
            console.error('WebSocket Error:', data.error);
            addMessage(`Error: ${data.error}`, false);
        }
    };

    // Implement reconnection with exponential backoff
    ws.onclose = (event) => {
        updateConnectionStatus('disconnected', 'Connection lost');
        
        if (reconnectAttempts < MAX_RECONNECT_ATTEMPTS) {
            // Calculate delay with exponential backoff
            const delay = Math.min(RECONNECT_DELAY * Math.pow(2, reconnectAttempts), 30000);
            updateConnectionStatus('reconnecting', 
                `Reconnecting in ${delay/1000} seconds...`);
            setTimeout(initializeWebSocket, delay);
            reconnectAttempts++;
        } else {
            // Max retries reached
            updateConnectionStatus('disconnected', 
                'Connection failed. Please refresh the page.');
        }
    };

    // Handle connection errors
    ws.onerror = (error) => {
        console.error('WebSocket error:', error);
        updateConnectionStatus('error', 'Connection error');
    };
}
```

Let’s break down the key features:

1.  **Connection Management**:
    -   Handles both secure (`wss:`) and non-secure (`ws:`) connections
    -   Closes existing connections before creating new ones
    -   Includes the chat session ID in the connection URL
    -   Maintains connection state for the UI
2.  **Message Processing**:
    -   Parses incoming JSON messages
    -   Handles different message types (connection status, chat content, errors)
    -   Updates the UI with new messages
    -   Logs errors for debugging
3.  **Smart Reconnection**:
    -   Implements exponential backoff to prevent server flooding
    -   Starts with a 1-second delay, doubling with each attempt
    -   Caps maximum delay at 30 seconds
    -   Limits total reconnection attempts to 5
    -   Provides clear feedback during reconnection attempts
4.  **Error Handling**:
    -   Catches and logs WebSocket errors
    -   Updates UI to reflect connection state
    -   Provides user-friendly error messages
    -   Suggests actions when connection fails permanently

The client implementation features:

-   Protocol detection for secure/non-secure connections
-   Visual connection status updates
-   Automatic reconnection with exponential backoff
-   Maximum retry attempts to prevent infinite loops
-   Error handling and user feedback

#### Connection Status Management

Building user confidence in a real-time application is crucial, and one simple way to achieve this is by showing the connection state. Since we already track connection status in our WebSocket implementation, we can easily surface this to users:

```
function updateConnectionStatus(status, message) {
    const statusDot = document.querySelector('.status-dot');
    const statusText = document.querySelector('.status-text');
    
    // Update visual indicators
    statusDot.className = `status-dot ${status}`;
    statusText.textContent = message;
    
    // Disable/enable chat input based on connection status
    const chatInput = document.getElementById('message-input');
    const sendButton = document.querySelector('button[type="submit"]');
    const isConnected = status === 'connected';
    
    chatInput.disabled = !isConnected;
    sendButton.disabled = !isConnected;
}
```

This simple addition gives users immediate feedback about their connection state and automatically disables input when the connection is lost. It’s a small touch that makes the application feel more polished and reliable.

### [Chat Service and Message Processing](#chat-service-and-message-processing)[](#chat-service-and-message-processing)

When building a chat interface with the GenAI Platform, one of the interesting challenges is handling the streaming responses. The AI model generates text token by token, which raises some interesting questions:

1.  How might I transform these tiny chunks into a smooth reading experience?
2.  How can I maintain conversation context for meaningful interactions?
3.  How can I manage errors that might occur during streaming?
4.  What’s the best way to maintain a responsive user interface?

Here’s an approach we can take to address these challenges. Here we’ll implement a buffering strategy that collects tokens into meaningful chunks before sending them to the client, while also maintaining conversation history for context-aware responses.

#### Message Processing Approach

Rather than sending each token as it arrives, we can buffer the content and send it at natural breaking points. Here’s the implementation:

```
async processStream(stream, ws, sessionId) {
    const conversationHistory = this.conversations.get(sessionId);
    let currentChunk = [];
    let fullResponse = [];
    
    for await (const chunk of stream) {
        if (chunk.choices[0]?.delta?.content) {
            const content = chunk.choices[0].delta.content;
            currentChunk.push(content);
            
            // Send chunk at natural breaks (punctuation or word count)
            if (this.shouldSendChunk(content, currentChunk)) {
                const message = currentChunk.join('');
                fullResponse.push(message);
                ws.send(JSON.stringify({ content: message }));
                currentChunk = [];
            }
        }
    }
    
    // Store the complete response in conversation history
    if (fullResponse.length > 0) {
        conversationHistory.push({
            role: 'assistant',
            content: fullResponse.join('')
        });
    }
}
```

Let’s look at the key features:

1.  **Smart Chunking**:
    -   Collects tokens into meaningful chunks
    -   Sends updates at natural breaking points (punctuation or word count)
    -   Creates a smoother reading experience for users
2.  **Conversation History**:
    -   Maintains context for each chat session in memory
    -   Stores both user messages and AI responses during the session
    -   Enables more coherent and contextual interactions while connected
    -   Each session has its own independent history, which is cleared after 5 minutes of inactivity
3.  **Error Handling**:
    -   Monitors connection state during streaming
    -   Gracefully handles disconnections
    -   Provides clear error messages to users
    -   Prevents resource leaks
4.  **Resource Management**:
    -   Efficiently processes streaming responses
    -   Cleans up resources when connections close
    -   Maintains separate contexts for each session
    -   Balances memory usage with user experience

This implementation creates a robust chat experience that:

-   Feels responsive and natural to users
-   Maintains conversation context effectively
-   Handles errors gracefully
-   Scales well with multiple users

Of course, this is just one way to approach the problem. You might find other strategies that better suit your specific needs, such as different chunking strategies, alternative error handling approaches, or other ways to manage the conversation state.

[Running the Application](#running-the-application)[](#running-the-application)
-------------------------------------------------------------------------------

Reading about building a chat application is one thing, but there’s nothing quite like getting your hands dirty and trying it out yourself. I’ve made it easy to get started with just a few simple steps:

1.  Clone the repository and install dependencies:
    
    ```
    git clone https://github.com/wadewegner/do-genai-customchatbot.git
    cd do-genai-customchatbot
    npm install
    ```
    
2.  Create and configure your agent:
    
    -   Follow the [GenAI Platform Quickstart guide](https://docs.digitalocean.com/products/genai-platform/getting-started/quickstart/) to create your agent
    -   Make your agent’s endpoint public by following [these instructions](https://docs.digitalocean.com/products/genai-platform/how-to/manage-ai-agent/use-agent/#call-your-agents-endpoint-in-your-app)
    -   Copy your agent’s ID, key, and endpoint URL for the next step
3.  Set up your environment variables in `.env`:
    
    ```
    API_BASE=https://cluster-api.do-ai.run/v1
    AGENT_ID=your-agent-id
    AGENT_KEY=your-agent-key
    AGENT_ENDPOINT=your-agent-endpoint
    SESSION_SECRET=your-session-secret
    ```
    
4.  Start the server:
    
    ```
    npm start
    ```
    

Now you can open your browser to `http://localhost:3000` and start chatting! This is your chance to experiment with the application and customize it to your needs. Try modifying the chat interface, adjusting the message processing, or adding new features – the possibilities are endless with the GenAI Platform’s flexibility.

When you’re ready to share your creation with the world, DigitalOcean’s App Platform is a great place to deploy and run your application publicly.

[Beyond the Chat Bot](#beyond-the-chat-bot)[](#beyond-the-chat-bot)
-------------------------------------------------------------------

While this example demonstrates a chat interface, the GenAI Platform’s capabilities extend far beyond this use case. You could:

-   Build custom knowledge base assistants using RAG pipelines
-   Create content generation tools with custom models
-   Implement code analysis and generation systems
-   Develop language translation services
-   Add AI capabilities to existing applications

What excites me most about building with the GenAI Platform is how it lets you focus on the creative aspects of AI development. The chat application we built here is just scratching the surface – it’s a foundation you can build upon, experiment with, and make entirely your own. I hope this walkthrough has sparked some ideas for your own projects. Now it’s your turn to take these patterns and create something amazing!

#### [Source](https://www.digitalocean.com/community/tutorials/custom-realtime-chat-application)

<br/>
---
