---
title: "Introducing EasyREST"
date: 2025-03-19
---

Hey everyone,

I’m excited to share my new open-source project, **EasyREST**. If you’ve ever worked with PostgREST for simple CRUD operations and thought, “I wish I could easily switch between SQLite, MySQL/MariaDB, PostgreSQL, and more,” then this project is for you!

## What Is EasyREST?

EasyREST is a lightweight and secure REST API server that gives you one unified interface for virtually any relational database. Instead of writing separate API layers for each database, EasyREST lets you connect to multiple databases through a single API with built-in support for authentication and context-aware queries.

## Why I Built It

I loved the simplicity of PostgREST for small projects, but as my applications grew, I needed something more flexible. Managing different types of databases became a hassle. I built EasyREST to simplify database interactions by:

- **Supporting Multiple Databases:** Out of the box, it works with SQLite and can be extended to MySQL/MariaDB, PostgreSQL, and more.
- **Context-Aware Queries:** It automatically injects context variables (like timezone, headers, JWT claims, HTTP method, and more) into your SQL queries. This makes it easier to write dynamic, secure queries without the extra boilerplate.
- **Easy Plugin System:** Using Hashicorp’s plugin framework, adding support for a new database is straightforward.

## How It Works

### Direct Variable Substitution

Instead of using complex SQL constructs like CTEs, EasyREST performs direct substitution of context variables in your queries. For example:

- **MySQL:** Variables with the prefix `@erctx_*` for context and `@request_*` for request data.
- **PostgreSQL:** Uses `set_config` to assign context variables.

This means you can write queries that automatically include things like the user’s timezone or JWT token claims.

### Automatic API Schema Generation

EasyREST can automatically generate a Swagger 2.0 schema for your database. This makes it super easy to integrate with tools like Swagger UI, so you can quickly see what endpoints are available and how to use them.

Here’s a simplified example of a Swagger schema for a `users` table:  

```
swagger: '2.0'
info:
  title: EasyRest API
  version: v0.3.0
host: localhost:8080
basePath: /api/test
schemes:
  - http
produces:
  - application/json
consumes:
  - application/json
securityDefinitions:
  jwtToken:
    flow: password
    scopes: {}
    tokenUrl: http://auth.example.com/token
    type: oauth2
definitions:
  test:
    properties:
      ID:
        type: integer
        x-nullable: true
      NAME:
        type: string
        x-nullable: true
    type: object
  users:
    properties:
      id:
        readOnly: true
        type: integer
        x-nullable: true
      name:
        type: string
        x-nullable: true
      to_update:
        type: integer
        x-nullable: true
    type: object
paths:
  /test/:
    get:
      description: Retrieve rows from the test
      parameters:
        # where params like in GET /users/
      responses:
        '200':
          description: Successful response
          schema:
            items:
              $ref: '#/definitions/test'
            type: array
      security:
        - jwtToken: []
      summary: Get test
  /users/:
    delete:
      description: Delete rows from users
      parameters:
        # where params like in GET
      responses:
        '204':
          description: Rows deleted
      security:
        - jwtToken: []
      summary: Delete rows from users
    get:
      description: Retrieve rows from the users
      parameters:
        - collectionFormat: csv
          description: Comma-separated list of fields
          enum:
            - to_update
            - id
            - name
          in: query
          name: select
          required: false
          type: string
        - collectionFormat: csv
          description: Comma-separated ordering fields
          in: query
          name: ordering
          required: false
          type: string
        - description: Maximum number of records to return
          in: query
          name: limit
          required: false
          type: integer
        - description: Number of records to skip
          in: query
          name: offset
          required: false
          type: integer
        - description: Filter for field 'to_update' with operator lt
          in: query
          name: where.lt.to_update
          required: false
          type: integer
        - description: Filter for field 'to_update' with operator lte
          in: query
          name: where.lte.to_update
          required: false
          type: integer
        - description: Filter for field 'to_update' with operator is
          in: query
          name: where.is.to_update
          required: false
          type: integer
        - collectionFormat: csv
          description: Filter for field 'to_update' with operator in
          in: query
          name: where.in.to_update
          required: false
          type: integer
        - description: Filter for field 'to_update' with operator eq
          in: query
          name: where.eq.to_update
          required: false
          type: integer
        - description: Filter for field 'to_update' with operator neq
          in: query
          name: where.neq.to_update
          required: false
          type: integer
        - description: Filter for field 'to_update' with operator gt
          in: query
          name: where.gt.to_update
          required: false
          type: integer
        - description: Filter for field 'to_update' with operator gte
          in: query
          name: where.gte.to_update
          required: false
          type: integer
        - description: Filter for field 'id' with operator gte
          in: query
          name: where.gte.id
          required: false
          type: integer
        - description: Filter for field 'id' with operator eq
          in: query
          name: where.eq.id
          required: false
          type: integer
        - description: Filter for field 'id' with operator neq
          in: query
          name: where.neq.id
          required: false
          type: integer
        - description: Filter for field 'id' with operator gt
          in: query
          name: where.gt.id
          required: false
          type: integer
        - collectionFormat: csv
          description: Filter for field 'id' with operator in
          in: query
          name: where.in.id
          required: false
          type: integer
        - description: Filter for field 'id' with operator lt
          in: query
          name: where.lt.id
          required: false
          type: integer
        - description: Filter for field 'id' with operator lte
          in: query
          name: where.lte.id
          required: false
          type: integer
        - description: Filter for field 'id' with operator is
          in: query
          name: where.is.id
          required: false
          type: integer
        - description: Filter for field 'name' with operator like
          in: query
          name: where.like.name
          required: false
          type: string
        - description: Filter for field 'name' with operator ilike
          in: query
          name: where.ilike.name
          required: false
          type: string
        - description: Filter for field 'name' with operator eq
          in: query
          name: where.eq.name
          required: false
          type: string
        - description: Filter for field 'name' with operator neq
          in: query
          name: where.neq.name
          required: false
          type: string
        - description: Filter for field 'name' with operator is
          in: query
          name: where.is.name
          required: false
          type: string
        - collectionFormat: csv
          description: Filter for field 'name' with operator in
          in: query
          name: where.in.name
          required: false
          type: string
      responses:
        '200':
          description: Successful response
          schema:
            items:
              $ref: '#/definitions/users'
            type: array
      security:
        - jwtToken: []
      summary: Get users
    patch:
      description: Update existing rows in users
      parameters:
        # where params like in GET
        - description: Partial update of a users object
          in: body
          name: body
          required: true
          schema:
            $ref: '#/definitions/users'
      responses:
        '200':
          description: Rows updated
          schema:
            properties:
              updated:
                type: integer
            type: object
      security:
        - jwtToken: []
      summary: Update rows in users
    post:
      description: Insert new rows into users
      parameters:
        - description: Array of users objects
          in: body
          name: body
          required: true
          schema:
            items:
              $ref: '#/definitions/users'
            type: array
      responses:
        '201':
          description: Rows created
      security:
        - jwtToken: []
      summary: Create rows in users
```

## Real-World Usage

### Multi-Database Connections

Imagine having two databases:

- **Test Database (`ER_DB_TEST`):** A simple SQLite database with a `users` table.
- **Orders Database (`ER_DB_ORDERS`):** Another SQLite database with an `orders` table.

Set your environment variables like this:  

```
export ER_DB_TEST="sqlite://./test.db"
export ER_DB_ORDERS="sqlite://./orders.db"
export ER_TOKEN_SECRET="your-secret"
export ER_TOKEN_USER_SEARCH="sub"
export ER_DEFAULT_TIMEZONE="GMT"
```

EasyREST will automatically route your API requests to the correct database based on these settings.

### Example API Calls

**Creating a User:**  

```
curl -X POST "http://localhost:8080/api/test/users/" 
  -H "Authorization: Bearer $TOKEN" 
  -H "Content-Type: application/json" 
  -d '[{"name": "Alice", "to_update": 0}]'
```

**Deleting a User:**  

```
curl -X DELETE "http://localhost:8080/api/users/?where.eq.name=Alice" 
  -H "Authorization: Bearer $TOKEN"
```

**Updating a User:**  

```
curl -X PATCH "http://localhost:8080/api/users/?where.eq.id=1" 
  -H "Authorization: Bearer $TOKEN" 
  -H "Content-Type: application/json" 
  -d '{"name": "Alice Updated"}'
```

## Performance and Security

EasyREST is built for production use. While adding features like token validation and context handling introduces a small overhead, the performance remains efficient. It’s secure by design—leveraging JWT for authentication and ensuring that every request carries all the necessary context for safe database operations.

## Wrapping Up

I built EasyREST to create a tool that scales with your projects—a flexible, secure API that simplifies working with multiple databases. Whether you’re building a simple project with SQLite or a more complex system with MySQL and PostgreSQL, EasyREST has got you covered.

I’d love for you to check it out, contribute, and share your feedback. Let’s make building database-driven applications simpler together!

Happy coding!

## Resources

- GitHub Repository
- Installation Guide
- MySQL Plugin
- PostgreSQL Plugin

Go to Source
