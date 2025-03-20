---
title: "How to Set Up a Web Server with Rust and Actix"
date: 2025-01-03T00:00:00.000Z
draft: false
type: posts
categories: 
- saas,rust
---
# How to Set Up a Web Server with Rust and Actix

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

In this tutorial, you’ll learn how to build a web server with `Rust` and the `Actix-web` framework on a [DigitalOcean Droplet](/products/droplets). You’ll set up a basic server and create routes to manage a simple list of Todo items, supporting common RESTful operations like retrieving and creating Todos.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

Before you begin, ensure you have the following:

-   A [DigitalOcean Cloud account](https://cloud.digitalocean.com/) to spin up a Droplet.
-   An [Ubuntu Droplet](https://cloud.digitalocean.com/droplets/new) up and running.
-   [Rust](https://www.rust-lang.org/) installed on the Droplet.
-   Familiarity with [Rust basics](https://www.rust-lang.org/learn), including variables, functions, and traits.

[Step 1 - Setting up the Ubuntu Droplet](#step-1-setting-up-the-ubuntu-droplet)[](#step-1-setting-up-the-ubuntu-droplet)
------------------------------------------------------------------------------------------------------------------------

1.  **SSH into your DigitalOcean Droplet**: Open your terminal and use the following command to SSH into your Droplet. Replace `your_username` with your actual username and `your_droplet_ip` with the IP address of your Droplet:
    
    ```
    ssh your_username@your_droplet_ip
    ```
    
2.  **Update the package list**: Once logged in, update the package list to ensure you have the latest information on available packages:
    
    ```
    sudo apt update
    ```
    
3.  **Install Rust**: To install Rust, use the following command, which downloads and runs the Rust installation script:
    
    ```
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    ```
    
    Follow the on-screen instructions to complete the installation. After installation, you may need to restart your terminal or run:
    
    ```
    source $HOME/.cargo/env
    ```
    
4.  **Install necessary libraries**: Install the required libraries for building Rust applications. You can install `build-essential` and `libssl-dev` using:
    
    ```
    sudo apt install build-essential libssl-dev
    ```
    
5.  **Verify Rust installation**: To confirm that Rust has been installed correctly, check the version:
    
    ```
    rustc --version
    ```
    
6.  **Install Cargo (Rust package manager)**: Cargo is installed automatically with Rust. You can verify its installation by checking the version:
    
    ```
    cargo --version
    ```
    

[Step 2 - Setting Up the Project](#step-2-setting-up-the-project)[](#step-2-setting-up-the-project)
---------------------------------------------------------------------------------------------------

Let’s create a new rust project `sammy_todos` using `cargo`

```
cargo new sammy_todos
```

Next, find the `Cargo.toml` file. Navigate to the root directory of your Rust project. The `Cargo.toml` file is located in the same directory where you created your project (e.g., `sammy_todos`). You can use the following command to change to your project directory:

```
cd sammy_todos/
ls
```

Below are the Rust project files files which are automatically generated:

```
OutputCargo.toml  src
```

Add `actix-web` and `serde` as dependencies in `Cargo.toml`. Here, `serde` is needed for Serialization and Deserialization.

```
vi Cargo.toml
```

And add the following lines under the `dependencies`.

```
[dependencies]
actix-web = "4.0"
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
```

Now to download and compile dependencies use the below command:

```
cargo build
```

[Step 3 - Creating a Basic Server](#step-3-creating-a-basic-server)[](#step-3-creating-a-basic-server)
------------------------------------------------------------------------------------------------------

In `main.rs`, add the following lines of code to set up a basic server. You must navigate to the `src` directory inside your Project.

main.rs

```
use actix_web::{get, App, HttpServer, HttpResponse, Responder,web};

#[get("/")]
async fn index() -> impl Responder {
    HttpResponse::Ok().body("Welcome to the Sammy Todo API!")
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .service(index)
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}
```

Run the server:

```
cargo run
```

To test the server, open a new terminal session and run the following command:

```
curl http://127.0.0.1:8080
```

You should get the following response:

```
OutputWelcome to the Sammy Todo API!
```

[Step 4 - Adding a Todo Model](#step-4-adding-a-todo-model)[](#step-4-adding-a-todo-model)
------------------------------------------------------------------------------------------

Todo model is needed to serialize and deserialize the request and response. Add the following lines of code inside the `main.rs` file.

main.rs

```
use serde::{Serialize, Deserialize};

#[derive(Serialize, Deserialize)]
struct Todo {
    id: i32,
    title: String,
    completed: bool,
}
```

[Step 5 - Implement Create todo](#step-5-implement-create-todo)[](#step-5-implement-create-todo)
------------------------------------------------------------------------------------------------

This API creates todo, and this is a dummy API that returns your todo only, but you can implement it with databases to make it persistent

main.rs

```
#[get("/todos")]
async fn create_todo(todo: web::Json<Todo>) -> impl Responder {
    HttpResponse::Ok().body(serde_json::to_string(&todo).unwrap())
}
```

[Step 6 - Implement Get todo](#step-6-implement-get-todo)[](#step-6-implement-get-todo)
---------------------------------------------------------------------------------------

This API returns todo by id:

main.rs

```
#[get("/todos/{id}")]
async fn get_todo(id: web::Path<i32>) -> impl Responder {
    let todo = Todo {
        id: id.into_inner(),
        title: "Learn Actix-Web".to_string(),
        completed: false,
    };
    HttpResponse::Ok().body(serde_json::to_string(&todo).unwrap())
}
```

[Bind the services with our server](#bind-the-services-with-our-server)[](#bind-the-services-with-our-server)
-------------------------------------------------------------------------------------------------------------

This is how the rust server `main.rs` would look like:

main.rs

```
use actix_web::{get, App, HttpServer, HttpResponse, Responder, web};

#[get("/")]
async fn index() -> impl Responder {
    HttpResponse::Ok().body("Welcome to the Sammy Todo API!")
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .service(index)
            .service(get_todo)
            .service(create_todo)
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}

use serde::{Serialize, Deserialize}; // Added Deserialize to imports

#[derive(Serialize, Deserialize)] // Ensure Deserialize is imported
struct Todo {
    id: i32,
    title: String,
    completed: bool,
}

#[get("/todos")]
async fn create_todo(todo: web::Json<Todo>) -> impl Responder {
    HttpResponse::Ok().body(serde_json::to_string(&todo).unwrap())
}

#[get("/todos/{id}")]
async fn get_todo(id: web::Path<i32>) -> impl Responder {
    let todo = Todo {
        id: id.into_inner(),
        title: "Learn Actix-Web".to_string(),
        completed: false,
    };
    HttpResponse::Ok().body(serde_json::to_string(&todo).unwrap())
}
```

Now, you can test it by sending a `POST` request with `curl`:

```
curl -X POST -H "Content-Type: application/json" -d '{"title": "Buy groceries"}' http://127.0.0.1:8080/todos
```

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

Congratulations! You have successfully set up a basic web server using Rust and Actix. This tutorial taught you to create endpoints to retrieve and create Todo items, providing a solid foundation for building more complex applications. To enhance your app further, consider integrating a database solution for data persistence, allowing you to store and manage your Todo items effectively. Keep exploring, and happy coding!

#### [Source](https://www.digitalocean.com/community/tutorials/how-to-setup-a-webserver-using-rust-actix)

<br/>
---
