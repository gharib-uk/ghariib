---
title: "From Junior to Senior Developer: 4 Essential System Design Patterns 📈"
date: 2025-01-22
---

As systems grow more complex, understanding key design patterns becomes crucial for leveling up your skill set and career. In this brief guide, we'll explore four essential patterns that are shaping modern software architecture. Whether you're building scalable systems or preparing for senior role interviews, these patterns will be an important part of your tool belt.

## 1\. Event-Driven Architecture

Event-Driven Architecture has become the backbone of real-time, scalable applications. Instead of services directly calling each other, they communicate through events, leading to better scalability and maintainability.

### Key Components:

- Event Publishers (Producers)
- Event Subscribers (Consumers)
- Event Broker/Message Bus

Here's a simple example using Node.js and EventEmitter:  

```
import { EventEmitter } from 'events';

// Create an event bus
const orderEventBus = new EventEmitter();
// in production, you may want to use a singleton
// or a more robust event bus like Redis or RabbitMQ

type Order = {
  id: number;
  customerId: number;
  amount: number;
};

type CreateOrder = Omit<Order, 'id'>;

// Event name constants to prevent typos and enable better type checking
const EVENT_NAMES = {
  ORDER_CREATED: 'order.created',
  ORDER_FAILED: 'order.failed',
} as const;

// Event payload types
type OrderCreatedEvent = {
  type: typeof EVENT_NAMES.ORDER_CREATED;
  payload: Order;
  timestamp: Date;
};

type OrderFailedEvent = {
  type: typeof EVENT_NAMES.ORDER_FAILED;
  error: string;
  orderData: CreateOrder;
  timestamp: Date;
};

// Order Service (Publisher)
class OrderService {
  private lastOrderId = 0;
  private readonly eventBus: EventEmitter;

  // constructor(private readonly eventBus: EventEmitter) {}
  constructor(eventBus: EventEmitter) {
    this.eventBus = eventBus;
  }

  private generateOrderId(): number {
    // Simple incremental ID generation 
    // in production use UUID or other unique ID
    this.lastOrderId += 1;
    return this.lastOrderId;
  }

  async createOrder(orderData: CreateOrder): Promise<Order> {
    try {
      // Validate order data
      if (orderData.amount <= 0) {
        throw new Error('Order amount must be greater than 0');
      }

      // Create order with generated ID
      const order: Order = {
        id: this.generateOrderId(),
        ...orderData,
      };

      // Simulate async order processing
      await new Promise((resolve) => {
        setTimeout(resolve, 100);
      });

      // Emit success event with typed payload
      const event: OrderCreatedEvent = {
        type: EVENT_NAMES.ORDER_CREATED,
        payload: order,
        timestamp: new Date(),
      };

      this.eventBus.emit(event.type, event);
      return order;
    } catch (error) {
      // Handle errors and emit failure event
      const failureEvent: OrderFailedEvent = {
        type: EVENT_NAMES.ORDER_FAILED,
        error: error instanceof Error ? error.message : 'Unknown error',
        orderData,
        timestamp: new Date(),
      };

      this.eventBus.emit(failureEvent.type, failureEvent);
      throw error;
    }
  }
}

// Notification Service (Subscriber)
orderEventBus.on(EVENT_NAMES.ORDER_CREATED, (event: OrderCreatedEvent) => {
  console.log('Order created:', event.payload);
  // Send notification logic
});

// Analytics Service (Subscriber)
orderEventBus.on(EVENT_NAMES.ORDER_FAILED, (event: OrderFailedEvent) => {
  console.error('Order failed:', event.error);
  // Record analytics logic
});

// Usage example
export async function main() {
  console.log('Starting order processing...');
  const orderService = new OrderService(orderEventBus);
  await orderService.createOrder({ customerId: 412, amount: 99.99 });
  console.log('Order processing complete.');
}
```

## 2\. Circuit Breaker Pattern

The Circuit Breaker prevents cascading failures in distributed systems. Think of it as a safety switch that trips when a service is struggling, providing fallback behavior and preventing system overload.  

```
type AsyncFunction = () => Promise<unknown>;

class CircuitBreaker {
  private failures = 0;
  private lastFailureTime = 0;
  private readonly threshold: number = 5;
  private readonly resetTimeout: number = 60000; // 1 minute
  private state: 'CLOSED' | 'OPEN' | 'HALF_OPEN' = 'CLOSED';

  async execute(operation: AsyncFunction, fallback: AsyncFunction) {
    if (this.state === 'OPEN') {
      if (Date.now() - this.lastFailureTime >= this.resetTimeout) {
        this.state = 'HALF_OPEN';
      } else {
        return await fallback();
      }
    }

    try {
      const result = await operation();
      this.onSuccess();
      return result;
    } catch (error) {
      console.error('Operation failed:', error);
      return this.onFailure(fallback);
    }
  }

  private onSuccess(): void {
    this.failures = 0;
    this.state = 'CLOSED';
  }

  private async onFailure(fallback: () => Promise<unknown>) {
    this.failures += 1;
    this.lastFailureTime = Date.now();

    if (this.failures >= this.threshold) {
      this.state = 'OPEN';
    }

    return await fallback();
  }
}

// Usage example
const circuitBreaker = new CircuitBreaker();
export async function main() {
  const userId = '123';
  return await circuitBreaker.execute(
    async () => {
      // Real API call
      const response = await fetch(`/api/users/${userId}`);
      return response.json();
    },
    async () => {
      // Fallback response

      // Simulate fallback processing
      await new Promise((resolve) => {
        setTimeout(resolve, 100);
      });

      return { id: userId, name: 'Unknown', cached: true };
    },
  );
}
```

## 3\. API Gateway Pattern

The API Gateway serves as a single entry point for all clients, handling cross-cutting concerns like authentication, rate limiting, and request routing.

For example, we use this pattern at HazelBase to manage requests to our phone caller ID service from through our main API that we provide.  

```
class ApiGateway {
  private readonly rateLimits = new Map<string, number>();
  private readonly maxRequestsPerMinute: number = 60;

  // have client2 already be over the rate limit
  constructor() {
    this.rateLimits.set('client2', 100);
  }

  async handleRequest(req: Request): Promise<Response> {
    const clientId = this.getClientId(req);

    if (!this.checkRateLimit(clientId)) {
      return new Response('Rate limit exceeded', { status: 429 });
    }

    if (!this.authenticate(req)) {
      return new Response('Unauthorized', { status: 401 });
    }

    try {
      return await this.routeRequest(req);
    } catch (error) {
      console.error('Internal Server Error:', error);
      return new Response('Internal Server Error', { status: 500 });
    }
  }

  private getClientId(req: Request): string {
    return req.headers.get('X-Client-ID') || 'anonymous';
  }

  private checkRateLimit(clientId: string): boolean {
    const currentRequests = this.rateLimits.get(clientId) || 0;
    if (currentRequests >= this.maxRequestsPerMinute) {
      return false;
    }
    this.rateLimits.set(clientId, currentRequests + 1);
    return true;
  }

  private authenticate(req: Request): boolean {
    const token = req.headers.get('Authorization');
    // Implement authentication logic
    return Boolean(token);
  }

  private async routeRequest(req: Request): Promise<Response> {
    const path = new URL(req.url).pathname;

    switch (path) {
      case '/api/users':
        return await this.forwardToUserService(req);
      case '/api/orders':
        return await this.forwardToOrderService(req);
      default:
        return new Response('Not Found', { status: 404 });
    }
  }

  private async forwardToUserService(req: Request): Promise<Response> {
    // Implement actual service call
    console.log('Forwarding to order service...', req.url);
    return new Response(JSON.stringify({ id: 1, name: 'John Doe' }));
  }

  private async forwardToOrderService(req: Request): Promise<Response> {
    // Implement actual service call
    console.log('Forwarding to order service...', req.url);
    return new Response(JSON.stringify({ orderId: 1, status: 'pending' }));
  }
}

// Usage Example
const gateway = new ApiGateway();
export async function main() {
  // Simulate client requests
  const requests = [
    // Successful request
    new Request('https://api.example.com/api/users', {
      headers: {
        Authorization: 'Bearer token123',
        'X-Client-ID': 'client1',
      },
    }),

    // Rate limited request
    new Request('https://api.example.com/api/orders', {
      headers: {
        Authorization: 'Bearer token123',
        'X-Client-ID': 'client2',
      },
    }),

    // Unauthorized request
    new Request('https://api.example.com/api/users', {
      headers: {
        'X-Client-ID': 'client3',
      },
    }),
  ];

  // Process requests
  for (const request of requests) {
    const response = await gateway.handleRequest(request);
    console.log(`${request.url}: ${response.status.toString()} ${await response.text()}`);
  }
}
```

## 4\. Database-Per-Service Pattern

This pattern ensures service independence by giving each service its own database. While it introduces some complexity, it's crucial for true service autonomy.

This is especially important as each service evolves independently. Originally at HazelBase, we used the same database type for the People data and Company data we provide. However as the size of each dataset grew (billions of records) and we updated how our users can search each of them, we had to change the database technologies we used and now the Company data uses an entirely different type of database from our People data. Since we followed this pattern, this update was thankfully painless.  

```
// Types and Interfaces
type User = {
  id: string;
  name: string;
  email: string;
};

type OrderItem = {
  productId: string;
  quantity: number;
  price: number;
};

type Order = {
  id: string;
  userId: string;
  items: OrderItem[];
  status: 'pending' | 'completed' | 'cancelled';
  createdAt: Date;
};

// Database Interfaces
type UserDB = {
  connect(): Promise<void>;
  findUser(id: string): Promise<User | null>;
  createUser(user: User): Promise<void>;
};

type OrderDB = {
  connect(): Promise<void>;
  findOrder(id: string): Promise<Order | null>;
  createOrder(order: Order): Promise<void>;
  findOrdersByUser(userId: string): Promise<Order[]>;
};

// Service Implementations
class UserService {
  private db: UserDB;

  constructor(db: UserDB) {
    this.db = db;
  }

  async getUser(id: string): Promise<User> {
    const user = await this.db.findUser(id);
    if (!user) {
      throw new Error('User not found');
    }
    return user;
  }

  async createUser(userData: Omit<User, 'id'>): Promise<User> {
    const user: User = {
      id: crypto.randomUUID(),
      ...userData,
    };
    await this.db.createUser(user);
    return user;
  }
}

class OrderService {
  private db: OrderDB;
  private userService: UserService;

  constructor(db: OrderDB, userService: UserService) {
    this.db = db;
    this.userService = userService;
  }

  async createOrder(userId: string, items: OrderItem[]): Promise<Order> {
    // Verify user exists first
    await this.userService.getUser(userId);

    const order: Order = {
      id: crypto.randomUUID(),
      userId,
      items,
      status: 'pending',
      createdAt: new Date(),
    };

    await this.db.createOrder(order);
    return order;
  }

  async getUserOrders(userId: string): Promise<Order[]> {
    // Verify user exists
    await this.userService.getUser(userId);
    return this.db.findOrdersByUser(userId);
  }
}

// Mock Database Implementations for Example
class MockUserDB implements UserDB {
  private users = new Map<string, User>();

  async connect(): Promise<void> {
    console.log('Connected to User DB');
  }

  async findUser(id: string): Promise<User | null> {
    return this.users.get(id) || null;
  }

  async createUser(user: User): Promise<void> {
    this.users.set(user.id, user);
  }
}

class MockOrderDB implements OrderDB {
  private orders = new Map<string, Order>();

  async connect(): Promise<void> {
    console.log('Connected to Order DB');
  }

  async findOrder(id: string): Promise<Order | null> {
    return this.orders.get(id) || null;
  }

  async createOrder(order: Order): Promise<void> {
    this.orders.set(order.id, order);
  }

  async findOrdersByUser(userId: string): Promise<Order[]> {
    return Array.from(this.orders.values()).filter((order) => order.userId === userId);
  }
}

// Usage Example
export async function main() {
  // Initialize services
  const userDb = new MockUserDB();
  const orderDb = new MockOrderDB();
  await userDb.connect();
  await orderDb.connect();

  const userService = new UserService(userDb);
  const orderService = new OrderService(orderDb, userService);

  try {
    // Create a new user
    const user = await userService.createUser({
      name: 'John Doe',
      email: 'john@example.com',
    });
    console.log('Created user:', user);

    // Create an order for the user
    const order = await orderService.createOrder(user.id, [
      { productId: 'prod1', quantity: 2, price: 29.99 },
      { productId: 'prod2', quantity: 1, price: 49.99 },
    ]);
    console.log('Created order:', order);

    // Fetch user's orders
    const userOrders = await orderService.getUserOrders(user.id);
    console.log('User orders:', userOrders);
  } catch (error) {
    console.error('Error:', error);
  }
}
```

## Key Takeaways

1. Event-Driven Architecture enables loose coupling and scalability
2. Circuit Breakers prevent cascade failures
3. API Gateways centralize cross-cutting concerns
4. Database-Per-Service ensures service autonomy

While these are just simple examples, these patterns are solving real problems in production systems today. When you encounter challenges in your projects, consider how applying these patterns may help.

Want to dive deeper? Check out the documentation for popular implementations like Netflix's Hystrix (Circuit Breaker) or Kong API Gateway.

Go to Source
