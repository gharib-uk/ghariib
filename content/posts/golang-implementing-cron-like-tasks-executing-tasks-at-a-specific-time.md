---
title: "Golang: Implementing Cron-Like Tasks / Executing Tasks at a Specific Time"
date: 2025-02-06
---

Scheduling tasks in Golang is a common requirement for automation, background jobs, and periodic tasks. This article explores different approaches, from simple time-based execution to robust scheduling libraries and cloud-native solutions.

![Am I A Joke To You?](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmh26xev08wbz82p77b19.png)

## 1\. **Raw Implementation Using `time` Package**

For simple task scheduling, you can use the `time` package in Go.

### **1.1 Using `time.AfterFunc` (One-time Delayed Execution)**

```
package main

import (
    "fmt"
    "time"
)

func main() {
    time.AfterFunc(3*time.Second, func() {
        fmt.Println("Executed after 3 seconds")
    })

    // Prevent the program from exiting immediately
    time.Sleep(5 * time.Second)
}
```

**Output:**  

```
Executed after 3 seconds
```

### **1.2 Using `time.Ticker` (Periodic Execution)**

```
package main

import (
    "fmt"
    "time"
)

func main() {
    ticker := time.NewTicker(2 * time.Second)
    defer ticker.Stop()

    for i := 0; i < 3; i++ {
        <-ticker.C
        fmt.Println("Task executed at:", time.Now())
    }
}
```

**Output:**  

```
Task executed at: 2025-02-07 12:00:00 +0000 UTC
Task executed at: 2025-02-07 12:00:02 +0000 UTC
Task executed at: 2025-02-07 12:00:04 +0000 UTC
```

**Pros:**

- Lightweight, built into Go
- No external dependencies

**Cons:**

- Requires manual handling of concurrency and persistence
- Limited to running inside the same process

## 2\. **Using `cron`\-like Libraries in Go**

For more complex scheduling needs, use existing cron libraries that offer better flexibility and reliability.

### **2.1 Using `robfig/cron` (Crontab Syntax Support)**

Install the library:  

```
go get github.com/robfig/cron/v3
```

Example:  

```
package main

import (
    "fmt"
    "time"

    "github.com/robfig/cron/v3"
)

func main() {
    c := cron.New()

    // Schedule a job every 5 seconds
    c.AddFunc("*/5 * * * * *", func() {
        fmt.Println("Cron job executed at:", time.Now())
    })

    c.Start()

    // Keep the main function running
    select {}
}
```

### **2.2 Using `gocron` (Simpler API for Recurring Jobs)**

Install:  

```
go get github.com/go-co-op/gocron
```

Example:  

```
package main

import (
    "fmt"
    "time"

    "github.com/go-co-op/gocron"
)

func main() {
    s := gocron.NewScheduler(time.UTC)

    s.Every(10).Seconds().Do(func() {
        fmt.Println("Scheduled task executed at:", time.Now())
    })

    s.StartAsync()

    // Prevent exit
    select {}
}
```

**Pros:**

- More robust than `time.Ticker`
- Supports standard cron expressions

**Cons:**

- Runs within the Go process (needs process monitoring)

## 3\. **Using System `cron` (Best for CLI Execution)**

Instead of running a Go process indefinitely, you can execute a Go script using Linux's `cron` daemon.

### **Example `crontab` Entry (Runs Every Minute)**

```
* * * * * /path/to/your-go-program
```

To set this up:

1. Build the Go program:

```
   go build -o mytask mytask.go
```

1. Add it to `crontab -e`:

```
   * * * * * /path/to/mytask
```

**Pros:**

- More stable, managed by the OS
- Easy to monitor and log

**Cons:**

- Requires additional setup
- Limited to CLI tasks

## 4\. **Cloud-Native Approaches**

If you're deploying to the cloud, managed solutions may be better.

### **4.1 Google Cloud Run Jobs**

- Deploy a container that runs on a schedule.
- Use Google Cloud Scheduler to trigger the job.

```
gcloud scheduler jobs create http my-job 
  --schedule="*/10 * * * *" 
  --uri="https://your-cloud-run-service-url"
```

### **4.2 AWS Lambda with EventBridge**

Create an AWS Lambda function and trigger it using EventBridge (formerly CloudWatch Events).

Example EventBridge rule (runs every 5 minutes):  

```
{
  "ScheduleExpression": "rate(5 minutes)"
}
```

### **4.3 Kubernetes CronJobs**

Example `cronjob.yaml` (Runs Every Hour):  

```
apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-cronjob
spec:
  schedule: "0 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: my-job
            image: my-go-image
          restartPolicy: OnFailure
```

**Pros:**

- Fully managed, scalable
- Works well in containerized environments

**Cons:**

- Requires cloud setup and configuration

## 5\. **Comparing the Options: What Should You Use?**

| **Approach** | **Best For** | **Pros** | **Cons** |
| --- | --- | --- | --- |
| `time.Ticker` | Simple recurring jobs | No dependencies, lightweight | Needs manual persistence & recovery |
| `robfig/cron` | Flexible job scheduling | Crontab syntax, well-tested | Runs inside app, process-dependent |
| System `cron` | CLI-based scheduled tasks | OS-managed, stable | Limited logging, debugging overhead |
| Cloud Run Jobs | Serverless scheduled tasks | Fully managed, scalable | Cloud-dependent, cold starts |
| Kubernetes CronJobs | Containerized jobs | Scalable, cloud-native | Requires Kubernetes setup |

## **Conclusion**

- If you need **simple, in-process scheduling**, use `time.Ticker`.
- If you want **crontab-like flexibility**, use `robfig/cron`.
- If your task is **system-wide**, use system `cron`.
- For **cloud-native workloads**, use **Cloud Run, Lambda, or Kubernetes CronJobs**.

By choosing the right approach, you can ensure that your scheduled tasks run efficiently and reliably.

Go to Source
