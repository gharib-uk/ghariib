---
title: "<div>Deploy a server using Go with GitLab + Google Cloud</div>"
date: 2025-01-29
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Deploying an application to the cloud often requires assistance from production or DevOps engineers. GitLab's Google Cloud integration empowers developers to handle deployments independently. In this tutorial, you'll learn how to deploy a server to Google Cloud in less than 10 minutes using Go. Whether you’re a solo developer or part of a large team, this setup allows you to deploy applications efficiently.

## You'll learn how to:

1. Create a new project in GitLab
2. Create a Go server utilizing `main.go`
3. Use the Google Cloud integration to create a Service account
4. Use the Google Cloud integration to create Cloud Run via a merge request
5. Access your newly deployed Go server
6. Clean up your environment

## Prerequisites

- Owner access on a Google Cloud Platform project
- Working knowledge of Golang
- Working knowledge of GitLab CI
- 10 minutes

## Step-by-step Golang server deployment to Google Cloud

### 1\. Create a new blank project in GitLab.

We decided to call our project `golang-cloud-run` for simplicity.

![Create a new blank project in GitLab](https://images.ctfassets.net/r9o86ar0p03f/2B16Vu0ntDofRfQA93sCth/80a80d1498f1d9278edf5f7f07d4c808/image9.png)

### 2\. Create a server utilizing this `main.go` demo.

Find the `main.go` demo here.

```
// Sample run-helloworld is a minimal Cloud Run service.
package main

import (
	"fmt"
	"log"
	"net/http"
	"os"
)

func main() {
	log.Print("starting server...")
	http.HandleFunc("/", handler)

	// Determine port for HTTP service.
	port := os.Getenv("PORT")
	if port == "" {
		port = "8080"
		log.Printf("defaulting to port %s", port)
	}

	// Start HTTP server.
	log.Printf("listening on port %s", port)
	if err := http.ListenAndServe(":"+port, nil); err != nil {
		log.Fatal(err)
	}
}

func handler(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	if name == "" {
		name = "World"
	}
	fmt.Fprintf(w, "Hello %s!n", name)
}
```

### 3\. Use the Google Cloud integration to create a Service account.

Navigate to **Operate > Google Cloud > Create Service account**.

![Golang tutorial - image 2](https://images.ctfassets.net/r9o86ar0p03f/4JzoDSNQns2lsTpymunzFZ/a376e3d710054a685eaf29fd59309549/image11.png)

### 4\. Configure the region you would like the Cloud Run instance deployed to.

![Golang tutorial - image10](https://images.ctfassets.net/r9o86ar0p03f/6Gu5KA7YoegDBnfRLIKZle/a675d9f617e02a3d1f0a3259332e069a/image10.png)

### 5\. Use the Google Cloud integration to configure Cloud Run via Merge Request.

![Golang tutorial - image4](https://images.ctfassets.net/r9o86ar0p03f/4FqDUPGS8Ee1uQIoZklYew/c01396e47b973513ad06a2b64fa498cb/image4.png)

### 6\. This will open a merge request. Immediately merge the MR.

![Golang tutorial - image6](https://images.ctfassets.net/r9o86ar0p03f/2LqZDXvP6g00RRdlnAe7rg/b931446567127375249a06d1f0c1a541/image6.png)

This merge request adds a CI/CD deployment job to your pipeline definition. In our case, this is also creating a pipeline definition, as we didn’t have one before.

**Note:** The CI/CD variables `GCP_PROJECT_ID`, `GCP_REGION`, `GCP_SERVICE_ACCOUNT`, `GCP_SERVICE_ACCOUNT_KEY` will all be automatically populated from the previous steps.

![Golang tutorial - image7](https://images.ctfassets.net/r9o86ar0p03f/1Mhk3mTDVpuxCitMiwSICG/51caac3d01839dffe29a2627ce334c11/image7.png)

### 7\. Voila! Check your pipeline and you will see you have successfully deployed to Google Cloud Run utilizing GitLab CI.

![Golang tutorial - image2](https://images.ctfassets.net/r9o86ar0p03f/4h6e6OwTqhvgmWa2H8zMzY/cfed174bf24a4c0c2e6e7297f679ecce/image2.png)

<br>

![Golang tutorial - image3](https://images.ctfassets.net/r9o86ar0p03f/5sRoDsJjcnmyJFwUHkh3Rj/e089f3c3a890d21c1247b49d3bc31e67/image3.png)

## 8\. Click the Service URL to view your newly deployed server.

Alternatively, you can navigate to **Operate > Environments** to see a list of deployments for your environments.

![Golang tutorial - image5](https://images.ctfassets.net/r9o86ar0p03f/7ClGU8XsMV1zNOA2GxRcNn/65999daafe259ad8e08fc7b47f0f980b/image5.png)

By clicking on the environment called **main**, you’ll be able to view a complete list of deployments specific to that environment.

![Golang tutorial - image8](https://images.ctfassets.net/r9o86ar0p03f/1w1J7vXmqzlr5aT0MLgqkH/3344886b16cbab3a17bd91b441dcbed8/image8.png)

## Next steps

To get started with developing your Go application, try adding another endpoint. For instance, in your `main.go` file, you can add a `/bye` endpoint as shown below (don’t forget to register the new handler function in main!):

```
func main() {
	log.Print("starting server...")

	http.HandleFunc("/", handler)
	http.HandleFunc("/bye", byeHandler)
```

```
func byeHandler(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	if name == "" {
		name = "World"
	}
	fmt.Fprintf(w, "Bye %s!n", name)
}
```

Your `main.go` file should now look something like this:

```
// Sample run-helloworld is a minimal Cloud Run service.
package main

import (
	"fmt"
	"log"
	"net/http"
	"os"
)

func main() {
	log.Print("starting server...")

	http.HandleFunc("/", handler)

	http.HandleFunc("/bye", byeHandler)

	// Determine port for HTTP service.
	port := os.Getenv("PORT")
	if port == "" {
		port = "8080"
		log.Printf("defaulting to port %s", port)
	}

	// Start HTTP server.
	log.Printf("listening on port %s", port)
	if err := http.ListenAndServe(":"+port, nil); err != nil {
		log.Fatal(err)
	}
}

func handler(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	if name == "" {
		name = "World"
	}
	fmt.Fprintf(w, "Hello %s!n", name)
}

func byeHandler(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	if name == "" {
		name = "World"
	}
	fmt.Fprintf(w, "Bye %s!n", name)
}
```

Push the changes to the repo, and watch the `deploy-to-cloud-run job` deploy the updates. Once it’s complete, go back to the Service URL and navigate to the `/bye` endpoint to see the new functionality in action.

## Clean up the environment

To prevent incurring charges on your Google Cloud account for the resources used in this tutorial, you can either delete the specific resources or delete the entire Google Cloud project. For detailed instructions, refer to the cleanup guide.

> Discover more tutorials like this in our Solutions Architecture area.

Deploying an application to the cloud often requires assistance from production or DevOps engineers. GitLab's Google Cloud integration empowers developers to handle deployments independently. In this tutorial, you'll learn how to deploy a server to Google Cloud in less than 10 minutes using Go. Whether you’re a solo developer or part of a large team, this setup allows you to deploy applications efficiently.

## You'll learn how to:

1. Create a new project in GitLab
2. Create a Go server utilizing `main.go`
3. Use the Google Cloud integration to create a Service account
4. Use the Google Cloud integration to create Cloud Run via a merge request
5. Access your newly deployed Go server
6. Clean up your environment

## Prerequisites

- Owner access on a Google Cloud Platform project
- Working knowledge of Golang
- Working knowledge of GitLab CI
- 10 minutes

## Step-by-step Golang server deployment to Google Cloud

### 1\. Create a new blank project in GitLab.

We decided to call our project `golang-cloud-run` for simplicity.

![Create a new blank project in GitLab](https://images.ctfassets.net/r9o86ar0p03f/2B16Vu0ntDofRfQA93sCth/80a80d1498f1d9278edf5f7f07d4c808/image9.png)

### 2\. Create a server utilizing this `main.go` demo.

Find the `main.go` demo here.

```
// Sample run-helloworld is a minimal Cloud Run service.
package main

import (
	"fmt"
	"log"
	"net/http"
	"os"
)

func main() {
	log.Print("starting server...")
	http.HandleFunc("/", handler)

	// Determine port for HTTP service.
	port := os.Getenv("PORT")
	if port == "" {
		port = "8080"
		log.Printf("defaulting to port %s", port)
	}

	// Start HTTP server.
	log.Printf("listening on port %s", port)
	if err := http.ListenAndServe(":"+port, nil); err != nil {
		log.Fatal(err)
	}
}

func handler(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	if name == "" {
		name = "World"
	}
	fmt.Fprintf(w, "Hello %s!n", name)
}
```

### 3\. Use the Google Cloud integration to create a Service account.

Navigate to **Operate > Google Cloud > Create Service account**.

![Golang tutorial - image 2](https://images.ctfassets.net/r9o86ar0p03f/4JzoDSNQns2lsTpymunzFZ/a376e3d710054a685eaf29fd59309549/image11.png)

### 4\. Configure the region you would like the Cloud Run instance deployed to.

![Golang tutorial - image10](https://images.ctfassets.net/r9o86ar0p03f/6Gu5KA7YoegDBnfRLIKZle/a675d9f617e02a3d1f0a3259332e069a/image10.png)

### 5\. Use the Google Cloud integration to configure Cloud Run via Merge Request.

![Golang tutorial - image4](https://images.ctfassets.net/r9o86ar0p03f/4FqDUPGS8Ee1uQIoZklYew/c01396e47b973513ad06a2b64fa498cb/image4.png)

### 6\. This will open a merge request. Immediately merge the MR.

![Golang tutorial - image6](https://images.ctfassets.net/r9o86ar0p03f/2LqZDXvP6g00RRdlnAe7rg/b931446567127375249a06d1f0c1a541/image6.png)

This merge request adds a CI/CD deployment job to your pipeline definition. In our case, this is also creating a pipeline definition, as we didn’t have one before.

**Note:** The CI/CD variables `GCP_PROJECT_ID`, `GCP_REGION`, `GCP_SERVICE_ACCOUNT`, `GCP_SERVICE_ACCOUNT_KEY` will all be automatically populated from the previous steps.

![Golang tutorial - image7](https://images.ctfassets.net/r9o86ar0p03f/1Mhk3mTDVpuxCitMiwSICG/51caac3d01839dffe29a2627ce334c11/image7.png)

### 7\. Voila! Check your pipeline and you will see you have successfully deployed to Google Cloud Run utilizing GitLab CI.

![Golang tutorial - image2](https://images.ctfassets.net/r9o86ar0p03f/4h6e6OwTqhvgmWa2H8zMzY/cfed174bf24a4c0c2e6e7297f679ecce/image2.png)

<br>

![Golang tutorial - image3](https://images.ctfassets.net/r9o86ar0p03f/5sRoDsJjcnmyJFwUHkh3Rj/e089f3c3a890d21c1247b49d3bc31e67/image3.png)

## 8\. Click the Service URL to view your newly deployed server.

Alternatively, you can navigate to **Operate > Environments** to see a list of deployments for your environments.

![Golang tutorial - image5](https://images.ctfassets.net/r9o86ar0p03f/7ClGU8XsMV1zNOA2GxRcNn/65999daafe259ad8e08fc7b47f0f980b/image5.png)

By clicking on the environment called **main**, you’ll be able to view a complete list of deployments specific to that environment.

![Golang tutorial - image8](https://images.ctfassets.net/r9o86ar0p03f/1w1J7vXmqzlr5aT0MLgqkH/3344886b16cbab3a17bd91b441dcbed8/image8.png)

## Next steps

To get started with developing your Go application, try adding another endpoint. For instance, in your `main.go` file, you can add a `/bye` endpoint as shown below (don’t forget to register the new handler function in main!):

```
func main() {
	log.Print("starting server...")

	http.HandleFunc("/", handler)
	http.HandleFunc("/bye", byeHandler)
```

```
func byeHandler(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	if name == "" {
		name = "World"
	}
	fmt.Fprintf(w, "Bye %s!n", name)
}
```

Your `main.go` file should now look something like this:

```
// Sample run-helloworld is a minimal Cloud Run service.
package main

import (
	"fmt"
	"log"
	"net/http"
	"os"
)

func main() {
	log.Print("starting server...")

	http.HandleFunc("/", handler)

	http.HandleFunc("/bye", byeHandler)

	// Determine port for HTTP service.
	port := os.Getenv("PORT")
	if port == "" {
		port = "8080"
		log.Printf("defaulting to port %s", port)
	}

	// Start HTTP server.
	log.Printf("listening on port %s", port)
	if err := http.ListenAndServe(":"+port, nil); err != nil {
		log.Fatal(err)
	}
}

func handler(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	if name == "" {
		name = "World"
	}
	fmt.Fprintf(w, "Hello %s!n", name)
}

func byeHandler(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	if name == "" {
		name = "World"
	}
	fmt.Fprintf(w, "Bye %s!n", name)
}
```

Push the changes to the repo, and watch the `deploy-to-cloud-run job` deploy the updates. Once it’s complete, go back to the Service URL and navigate to the `/bye` endpoint to see the new functionality in action.

## Clean up the environment

To prevent incurring charges on your Google Cloud account for the resources used in this tutorial, you can either delete the specific resources or delete the entire Google Cloud project. For detailed instructions, refer to the cleanup guide.

> Discover more tutorials like this in our Solutions Architecture area.

Go to Source
