---
title: "Shift-Left Testing with Testcontainers: Catching Bugs Early with Local Integration Tests"
date: 2025-03-19
---

Modern software development emphasizes speed and agility, making efficient testing crucial. DORA research reveals that elite teams thrive with both high performance and reliability. They can achieve 127x faster lead times, 182x more deployments per year, 8x lower change failure rates and most impressively, 2,293x faster recovery times after incidents. The secret sauce is they “shift left.” 

Shift-Left is a practice that moves integration activities like testing and security earlier in the development cycle, allowing teams to detect and fix issues before they reach production. By incorporating local and integration tests early, developers can prevent costly late-stage defects, accelerate development, and improve software quality. 

In this article, you’ll learn how integration tests can help you catch defects earlier in the development inner loop and how Testcontainers can make them feel as lightweight and easy as unit tests. Finally, we’ll break down the impact that shifting left integration tests has on the development process velocity and lead time for changes according to DORA metrics. 

## Real-world example: Case sensitivity bug in user registration

In a traditional workflow, integration and E2E tests are often executed in the outer loop of the development cycle, leading to delayed bug detection and expensive fixes. For example, if you are building a user registration service where users enter their email addresses, you must ensure that the emails are case-insensitive and not duplicated when stored. 

If case sensitivity is not handled properly and is assumed to be managed by the database, testing a scenario where users can register with duplicate emails differing only in letter case would only occur during E2E tests or manual checks. At that stage, it’s too late in the SDLC and can result in costly fixes.

By shifting testing earlier and enabling developers to spin up real services locally — such as databases, message brokers, cloud emulators, or other microservices — the testing process becomes significantly faster. This allows developers to detect and resolve defects sooner, preventing expensive late-stage fixes.

Let’s dive deep into this example scenario and how different types of tests would handle it.

### Scenario

A new developer is implementing a user registration service and preparing for production deployment.

**Code Example of the registerUser method**

```
async registerUser(email: string, username: string): Promise<User> {
    const existingUser = await this.userRepository.findOne({
        where: { 
            email: email          
        }
    });

    if (existingUser) {
        throw new Error("Email already exists");
    }
    ...
}

```

### The Bug

The `registerUser` method doesn’t handle case sensitivity properly and relies on the database or the UI framework to handle case insensitivity by default. So, in practice, users can register duplicate emails with both lower and upper letters  (e.g., `user@example.com` and `USER@example.com`).

### Impact

- Authentication issues arise because email case mismatches cause login failures.
- Security vulnerabilities appear due to duplicate user identities.
- Data inconsistencies complicate user identity management.

### Testing method 1: Unit tests. 

These tests only validate the code itself, so email case sensitivity verification relies on the database where SQL queries are executed. Since unit tests don’t run against a real database, they can’t catch issues like case sensitivity. 

### Testing method 2: End-to-end test or manual checks. 

These verifications will only catch the issue after the code is deployed to a staging environment. While automation can help, detecting issues this late in the development cycle delays feedback to developers and makes fixes more time-consuming and costly.

### Testing method 3: Using mocks to simulate database interactions with Unit Tests. 

One approach that could work and allow us to iterate quickly would be to mock the database layer and define a mock repository that responds with the error. Then, we could write a unit test that executes really fast:

```
test('should prevent registration with same email in different case', async () => {
  const userService = new UserRegistrationService(new MockRepository());
  await userService.registerUser({ email: 'user@example.com', password: 'password123' });
  await expect(userService.registerUser({ email: 'USER@example.com', password: 'password123' }))
    .rejects.toThrow('Email already exists');
});

```

In the above example, the User service is created with a mock repository that’ll hold an in-memory representation of the database, i.e. as a map of users. This mock repository will detect if a user has passed twice, probably using the username as a non-case-sensitive key, returning the expected error. 

Here, we have to code the validation logic in the mock, replicating what the User service or the database should do. Whenever the user’s validation needs a change, e.g. not including special characters, we have to change the mock too. Otherwise, our tests will assert against an outdated state of the validations. If the usage of mocks is spread across the entire codebase, this maintenance could be very hard to do.

To avoid that, we consider that integration tests with real representations of the services we depend on. In the above example,  using the database repository is much better than mocks, because it provides us with more confidence on what we are testing.

### Testing method 4: Shift-left local integration tests with Testcontainers 

Instead of using mocks, or waiting for staging to run the integration or E2E tests, we can detect the issue earlier.  This is achieved by enabling developers to run the integration tests for the project locally in the developer’s inner loop, using Testcontainers with a real PostgreSQL database.

#### Benefits

- **Time Savings**: Tests run in seconds, catching the bug early.
- **More Realistic Testing**: Uses an actual database instead of mocks.
- **Confidence in Production Readiness**: Ensures business-critical logic behaves as expected.

### Example integration test

First, let’s set up a PostgreSQL container using the Testcontainers library and create a userRepository to connect to this PostgreSQL instance:

```
let userService: UserRegistrationService;

beforeAll(async () => {
        container = await new PostgreSqlContainer("postgres:16")
            .start();
        
        dataSource = new DataSource({
            type: "postgres",
            host: container.getHost(),
            port: container.getMappedPort(5432),
            username: container.getUsername(),
            password: container.getPassword(),
            database: container.getDatabase(),
            entities: [User],
            synchronize: true,
            logging: true,
            connectTimeoutMS: 5000
        });
        await dataSource.initialize();
        const userRepository = dataSource.getRepository(User);
        userService = new UserRegistrationService(userRepository);
}, 30000);

```

Now, with initialized userService, we can use the registerUser method to test user registration with the real PostgreSQL instance:

```
test('should prevent registration with same email in different case', async () => {
  await userService.registerUser({ email: 'user@example.com', password: 'password123' });
  await expect(userService.registerUser({ email: 'USER@example.com', password: 'password123' }))
    .rejects.toThrow('Email already exists');
});

```

### Why This Works

- Uses a real PostgreSQL database via Testcontainers
- Validates case-insensitive email uniqueness
- Verifies email storage format

### How Testcontainers helps

Testcontainers modules provide preconfigured implementations for the most popular technologies, making it easier than ever to write robust tests. Whether your application relies on databases, message brokers, cloud services like AWS (via LocalStack), or other microservices, Testcontainers has a module to streamline your testing workflow.

With Testcontainers, you can also mock and simulate service-level interactions or use contract tests to verify how your services interact with others. Combining this approach with local testing against real dependencies, Testcontainers provides a comprehensive solution for local integration testing and eliminates the need for shared integration testing environments, which are often difficult and costly to set up and manage. To run Testcontainers tests, you need a Docker context to spin up containers. Docker Desktop ensures seamless compatibility with Testcontainers for local testing. 

### Testcontainers Cloud: Scalable Testing for High-Performing Teams

Testcontainers is a great solution to enable integration testing with real dependencies locally. If you want to take testing a step further — scaling Testcontainers usage across teams, monitoring images used for testing, or seamlessly running Testcontainers tests in CI — you should consider using Testcontainers Cloud. It provides ephemeral environments without the overhead of managing dedicated test infrastructure. Using Testcontainers Cloud locally and in CI ensures consistent testing outcomes, giving you greater confidence in your code changes. Additionally, Testcontainers Cloud allows you to seamlessly run integration tests in CI across multiple pipelines, helping to maintain high-quality standards at scale. Finally, Testcontainers Cloud is more secure and ideal for teams and enterprises who have more stringent requirements for containers’ security mechanisms.   

## Measuring the business impact of shift-left testing

As we have seen, shift-left testing with Testcontainers significantly improves defect detection rate and time and reduces context switching for developers. Let’s take the example above and compare different production deployment workflows and how early-stage testing would impact developer productivity. 

### Traditional workflow (shared integration environment)

#### Process breakdown:

The traditional workflow comprises writing feature code, running unit tests locally, committing changes, and creating pull requests for the verification flow in the outer loop. If a bug is detected in the outer loop, developers have to go back to their IDE and repeat the process of running the unit test locally and other steps to verify the fix. 

![blog without shift left](https://www.docker.com/app/uploads/2025/03/blog-without-shift-left-1110x389.png "- blog without shift left")

**Figure 1: Workflow of a traditional shared integration environment broken down by time taken for each step.**

**Lead Time for Changes (LTC):** It takes at least 1 to 2 hours to discover and fix the bug (more depending on CI/CD load and established practices). In the best-case scenario, it would take approximately 2 hours from code commit to production deployment. In the worst-case scenario, it may take several hours or even days if multiple iterations are required.

**Deployment Frequency (DF) Impact:** Since fixing a pipeline failure can take around 2 hours and there’s a daily time constraint (8-hour workday), you can realistically deploy only 3 to 4 times per day. If multiple failures occur, deployment frequency can drop further.

**Additional associated costs:** Pipeline workers’ runtime minutes and Shared Integration Environment maintenance costs.

**Developer Context Switching:** Since bug detection occurs about 30 minutes after the code commit, developers lose focus. This leads to an increased cognitive load after they have to constantly context switch, debug, and then context switch again.

### Shift-left workflow (local integration testing with Testcontainers)

#### Process breakdown:

The shift-left workflow is much simpler and starts with writing code and running unit tests. Instead of running integration tests in the outer loop, developers can run them locally in the inner loop to troubleshoot and fix issues. The changes are verified again before proceeding to the next steps and the outer loop. 

![blog with shift left](https://www.docker.com/app/uploads/2025/03/blog-with-shift-left-1110x467.png "- blog with shift left")

**Figure 2: Shift-Left Local Integration Testing with Testcontainers workflow broken down by time taken for each step. The feedback loop is much faster and saves developers time and headaches downstream.**

**Lead Time for Changes (LTC):** It takes less than 20 minutes to discover and fix the bug in the developers’ inner loop. Therefore, local integration testing enables at least 65% faster defect identification than testing on a Shared Integration Environment.  

**Deployment Frequency (DF) Impact:** Since the defect was identified and fixed locally within 20 minutes, the pipeline would run to production, allowing for 10 or more deployments daily.

**Additional associated costs:** 5 Testcontainers Cloud minutes are consumed.  

**Developer Context Switching:** No context switching for the developer, as tests running locally provide immediate feedback on code changes and let the developer stay focused within the IDE and in the inner loop.

### Key Takeaways  

|  | **Traditional Workflow (Shared Integration Environment)** | **Shift-Left Workflow (Local Integration Testing with Testcontainers)** | **Improvements and further references** |
| --- | --- | --- | --- |
| **Faster Lead Time for Changes (LTC** | Code changes validated in hours or days. Developers wait for shared CI/CD environments. | Code changes validated in minutes. Testing is immediate and local. | **\>65% Faster Lead Time for Changes (LTC)** – Microsoft reduced lead time from days to hours by adopting shift-left practices. |
| **Higher Deployment Frequency (DF)** | Deployment happens daily, weekly, or even monthly due to slow validation cycles. | Continuous testing allows multiple deployments per day. | **2x Higher Deployment Frequency**  – 2024 DORA Report shows shift-left practices more than double deployment frequency. Elite teams deploy 182x more often. |
| **Lower Change Failure Rate (CFR)** | Bugs that escape into production can lead to costly rollbacks and emergency fixes. | More bugs are caught earlier in CI/CD, reducing production failures. | **Lower Change Failure Rate –** IBM’s Systems Sciences Institute estimates defects found in production cost 15x more to fix than those caught early. |
| **Faster Mean Time to Recovery (MTTR)** | Fixes take hours, days, or weeks due to complex debugging in shared environments. | Rapid bug resolution with local testing. Fixes verified in minutes. | **Faster MTTR—**DORA’s elite performers restore service in less than one hour, compared to weeks to a month for low performers. |
| **Cost Savings** | Expensive shared environments, slow pipeline runs, high maintenance costs. | Eliminates costly test environments, reducing infrastructure expenses. | **Significant Cost Savings** – ThoughtWorks Technology Radar highlights shared integration environments as fragile and expensive. |

**Table 1: Summary of key metrics improvement by using shifting left workflow with local testing using Testcontainers**

### Conclusion

Shift-left testing improves software quality by catching issues earlier, reducing debugging effort, enhancing system stability, and overall increasing developer productivity. As we’ve seen, traditional workflows relying on shared integration environments introduce inefficiencies, increasing lead time for changes, deployment delays, and cognitive load due to frequent context switching. In contrast, by introducing Testcontainers for local integration testing, developers can achieve:

- **Faster feedback loops** – Bugs are identified and resolved within minutes, preventing delays.
- **More reliable application behavior** – Testing in realistic environments ensures confidence in releases.
- **Reduced reliance on expensive staging environments** – Minimizing shared infrastructure cuts costs and streamlines the CI/CD process.
- **Better developer flow state** – Easily setting up local test scenarios and re-running them fast for debugging helps developers stay focused on innovation.

Testcontainers provides an easy and efficient way to test locally and catch expensive issues earlier. To scale across teams,  developers can consider using Docker Desktop and Testcontainers Cloud to run unit and integration tests locally, in the CI, or ephemeral environments without the complexity of maintaining dedicated test infrastructure. Learn more about Testcontainers and Testcontainers Cloud in our docs. 

### Further Reading

- Sign up for a Testcontainers Cloud account.
- Follow the guide: Mastering Testcontainers Cloud by Docker: streamlining integration testing with containers
- Connect on the Testcontainers Slack.
- Get started with the Testcontainers guide.
- Learn about Testcontainers best practices.
- Learn about Spring Boot Application Testing and Development with Testcontainers
- Subscribe to the Docker Newsletter.
- Have questions? The Docker community is here to help.
- New to Docker? Get started.

​Engineering, Docker Desktop, Testcontainers
