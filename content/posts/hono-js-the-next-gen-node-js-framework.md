---
title: "Hono.js: The Next-Gen Node.js Framework"
date: 2025-01-06
---

## Why Learn Hono

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgaayv5o0kqvfyli5tqtf.png)

There are already many backend libraries and frameworks for Node.js on the market. I've used Nest.js for some small projects before. It has comprehensive functions and enables rapid project implementation. However, for my small projects, many of its features are really overkill, and it has a high level of encapsulation, leaving little freedom when writing code.

By chance, I came across Hono online. After reading its documentation:

- **Ultra-fast 🚀** - The router RegExpRouter is truly fast. It doesn't use linear loops. It's just quick.
- **Lightweight 🪶** - The `hono/tiny` preset is less than 12 kB. Hono has zero dependencies and only uses Web standard APIs.
- **Multi-runtime 🌍** - It works for Cloudflare Workers, Fastly Compute@Edge, Deno, Bun, Lagon, AWS Lambda, or Node.js. The same code runs on all platforms.
- **Capable 🔋** - Hono comes with built-in middleware, custom middleware, third-party middleware, and helpers. It's all-inclusive.
- **Pleasant DX 🛠️** - It has a super clean API and excellent TypeScript support. Now, we have "types".

## Use Cases

Hono is a simple web application framework, similar to Express, but without a frontend. It allows you to build larger applications when combined with middleware. Here are some use case examples:

- Building web interfaces
- Backend server proxy
- CDN frontends
- Edge applications
- Basic servers for libraries
- Full-stack applications

Great, let's start learning it.

## Hello World

https://hono.dev/docs/getting-started/basic

You can modify the port by setting the `port`.  

```
import { Hono } from 'hono';

const app = new Hono();

app.get('/', (c) => {
    return c.text('Hello Hono!');
});

export default app;
```

To execute:  

```
npm run dev
```

To access: http://localhost:8787

## Routes

### HTTP Methods

```
app.get('/', (c) => c.text('GET /'));
app.post('/', (c) => c.text('POST /'));
app.put('/', (c) => c.text('PUT /'));
app.delete('/', (c) => c.text('DELETE /'));
```

### Hierarchical Routes

```
const apiRoutes = app
   .basePath("/api")
   .route("/expenses", route1)
   .route("/", route2);
```

The previous `basePath("/api")` adds the `/api` prefix to all routes.  

```
export const route1 = new Hono()
   .post("/", async (c) => {
        return c.json({ });
    });
```

You can access the above route via http://localhost:8787/api/expenses.

## Requests

### Get Request Params and Query

```
app.get('/posts/:id', (c) => {
    const page = c.req.query('page');
    const id = c.req.param('id');
    return c.text(`You want see ${page} of ${id}`);
});
```

View the result: http://localhost:8787/posts/1?page=12

### Get the Content of the Request Body

```
app.put("/posts/:id{[0-9]+}", async (c) => {
    const data = await c.req.json();
    return c.json(data);
});
```

## Responses

Besides `text()`, there are many methods like `json()`, `html()`, `notFound()`, `redirect()`, etc., to make the request return different types of data. `html()` can directly return JSX.

### JSX

```
import type { FC } from 'hono/jsx';

const app = new Hono();

const Layout: FC = (props) => {
    return (
        <html>
            <body>{props.children}</body>
        </html>
    );
};

const Top: FC<{ messages: string[] }> = (props: {
    messages: string[]
}) => {
    return (HTTP Methods
        <Layout>
            <h1>Hello Hono!</h1>
            <ul>
                {props.messages.map((message) => {
                    return <li>{message}!!</li>;
                })}
            </ul>
        </Layout>
    );
};

app.get('/', (c) => {
    const messages = ['Good Morning', 'Good Evening', 'Good Night'];
    return c.html(<Top messages={messages} />);
});

export default app;
```

Just change the file extension to `.tsx` and you can directly write JSX, very React-like.

## Validators

Validators are implemented through `zod` and `@hono/zod-validator` to check if the requests sent from the client conform to the specified data format.  
**Installation**: `yarn add zod @hono/zod-validator`  
For example, if we need to verify that in a request, the data format sent by the client must be:  
`account: string; password: string`  

```
import { z } from "zod";
import { zValidator } from "@hono/zod-validator";

const loginSchema = z.object({
    account: z.string(),
    password: z.string(),
});

app.post("/login", zValidator("json", loginSchema), async (c) => {
    // do something
    const user = c.req.valid("json");
    return c.json({ });
});
```

Learn more from the `zod` documentation. And the IDE can directly show type hints for `user`.

## Leapcell: The Advanced Serverless Platform for Nodejs Hosting

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fg93vc6cxq39nu1zsl7s1.png)

Finally, let me introduce a platform that is very suitable for deploying Hono apps: Leapcell.

Leapcell is a serverless platform with the following characteristics:

1. **Multi-Language Support**
    
    - Develop with JavaScript, Python, Go, or Rust.
2. **Deploy unlimited projects for free**
    
    - Pay only for usage — no requests, no charges.
3. **Unbeatable Cost Efficiency**
    
    - Pay-as-you-go with no idle charges.
    - Example: $25 supports 6.94M requests at a 60ms average response time.
4. **Streamlined Developer Experience**
    
    - Intuitive UI for effortless setup.
    - Fully automated CI/CD pipelines and GitOps integration.
    - Real-time metrics and logging for actionable insights.
5. **Effortless Scalability and High Performance**
    
    - Auto-scaling to handle high concurrency with ease.
    - Zero operational overhead — just focus on building.

Explore more in the documentation!

Leapcell Twitter: https://x.com/LeapcellHQ

Go to Source
