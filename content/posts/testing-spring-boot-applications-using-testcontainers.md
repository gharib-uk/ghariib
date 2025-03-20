---
title: "Testing Spring Boot Applications Using Testcontainers"
date: 2025-01-04
categories: 
  - "general"
  - "linux"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "software"
---

Testing is a crucial part of software development, verifying that a system functions as intended. Developers create unit tests to validate the behavior of individual components, isolating them from external dependencies such as file systems, databases, message brokers, and third-party APIs. However, since many applications rely on these external components, developers write integration tests to \[…\]

Testing is a crucial part of software development, verifying that a system functions as intended. Developers create unit tests to validate the behavior of individual components, isolating them from external dependencies such as file systems, databases, message brokers, and third-party APIs. However, since many applications rely on these external components, developers write integration tests to ensure an application interacts correctly with its dependencies in a more complete environment.

Integration testing can be challenging if developers rely on shared environments, mock systems, or manual configuration of dependencies, all of which can lead to brittle tests and undetected issues in production.

Testcontainers for Java is an open-source Java library that provides lightweight, disposable containers as test fixtures, enabling developers to run real instances of databases, message brokers, web servers, and other dependencies during testing. By leveraging Docker, Testcontainers ensures that tests run in isolated and consistent environments, independent of the host machine’s configuration. Testcontainers integrates seamlessly with popular testing libraries like JUnit and TestNG, making it easy for developers to write integration tests.

This blog post covers the following topics:

- Getting started with Testcontainers

- Writing tests with Testcontainers and JUnit 5

- Testing Spring Data JPA repositories using Testcontainers

- Testing Spring Boot REST APIs using Testcontainers

Let’s get started by setting up Testcontainers. If you already know how to do that, feel free to skip ahead to the _Writing tests with Testcontainers and JUnit 5_ section.

## Getting started with Testcontainers

The Testcontainers library provides a higher-level API to manage the container life cycle, extract information about running containers, and more. While it is predominantly used for testing, Testcontainers can also be used for local development.

The Testcontainers library needs a container runtime to create the container instances. To find the supporting container runtimes, see this doc.

Let’s look at how you can use the Testcontainers API to create a Redis Docker container instance using a `GenericContainer` abstraction.

First, add the testcontainers dependency to your build file:

```
// Gradle build.gradle
testImplementation 'org.testcontainers:testcontainers:1.20.4'

//Maven pom.xml
<dependency>
   <groupId>org.testcontainers</groupId>
   <artifactId>testcontainers</artifactId>
   <version>1.20.4</version>
   <scope>test</scope>
</dependency>
```

Now, you can use the `GenericContainer` API to create a container from the `redis:7` Docker image exposing the container’s port 6379 to the host and use the container as follows:

```
import org.testcontainers.containers.GenericContainer;

GenericContainer<?> container = new GenericContainer<>("redis:7").withExposedPorts(6379);
container.start();
String host = container.getHost();
int hostPort = container.getMappedPort(6379);
System.out.println("Redis started on " + host + ":" + hostPort);
container.stop();
```

Testcontainers maps the exposed container ports to randomly available ports on the host machine so there won’t be any conflicts with other containers running in parallel. You can use the `container.getMappedPort(port)` method to get the mapped port on the host machine.

The `GenericContainer` provides a generic API for managing containers, but it doesn’t provide any container-specific methods. To streamline working with various technologies, Testcontainers offers technology-specific modules and additional API methods. You can see the full list of available Testcontainers modules here.

Now, let’s look at how you can use the Testcontainers PostgreSQL module and the additional methods it provides.

Add the Testcontainer `postgresql` module dependency to your build file:

```
// Gradle build.gradle
testImplementation 'org.testcontainers:postgresql:1.20.4'

//Maven pom.xml
<dependency>
   <groupId>org.testcontainers</groupId>
   <artifactId>postgresql</artifactId>
   <version>1.20.4</version>
   <scope>test</scope>
</dependency>
```

Now, you can use the `PostgreSQLContainer` class to create an instance of a PostgreSQL container:

```
PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:17");
postgres.start();

host = postgres.getHost();
hostPort = postgres.getMappedPort(5432);
//If you expose only one port, then you can use getFirstMappedPort()
hostPort = postgres.getFirstMappedPort();

// container-specific methods
String jdbcUrl = postgres.getJdbcUrl();
String username = postgres.getUsername();
String password = postgres.getPassword();
String databaseName = postgres.getDatabaseName();

postgres.stop();
```

`PostgreSQLContainer` abstracts the details of the container port number, database credentials, and readiness checks and provides you with a high-level API to interact with it. You can also see that this class provides additional methods to get information about the PostgreSQL container, such as `getJdbcUrl()` and `getDatabaseName()`.

**NOTE:** Even if you don’t explicitly destroy the containers by calling the `container.stop()` method, Testcontainers will automatically destroy the container upon exiting the JVM or by using the Moby Ryuk sidecar container behind the scenes.

Now that we’ve explored setting up the Testcontainers library to create, start, and stop Docker containers, as well as extract information about them, let’s see how to use Testcontainers for testing.

## Writing tests with Testcontainers and JUnit 5

Typically, the Testcontainers library is used for integration testing in conjunction with testing libraries like JUnit. The components, such as databases and message brokers, that are required for tests will be started before running any test cases​​ and destroyed after the test execution is completed.

You can use JUnit test life cycle callback methods to start and stop the containers:

```
import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.Test;
import org.testcontainers.containers.PostgreSQLContainer;

class TestcontainersWithJunit5Callbacks {
   static PostgreSQLContainer postgres = new PostgreSQLContainer("postgres:17");
  
   @BeforeAll
   static void beforeAll() {
       postgres.start();
       // configure your application to talk to the PostgreSQL container
   }

   @AfterAll
   static void afterAll() {
       postgres.stop();
   }

   @Test
   void test1() {
       // test code that uses postgres
   }

   @Test
   void test2() {
       // test code that uses postgres
   }
}
```

A common practice is starting the containers before test execution and destroying those containers after test execution is completed. The Testcontainers JUnit 5 Jupiter extension is created to simplify this process by using an annotation-based approach.

First, add the following Testcontainers `junit-jupiter` dependency:

```
// Gradle build.gradle
testImplementation 'org.testcontainers:junit-jupiter:1.20.4'

//Maven pom.xml
<dependency>
   <groupId>org.testcontainers</groupId>
   <artifactId>junit-jupiter</artifactId>
   <version>1.20.4</version>
   <scope>test</scope>
</dependency>
```

Now, you can write integration tests using the Testcontainers JUnit Jupiter extension:

```
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

@Testcontainers
class TestcontainersWithJuperExtension {
  
   @Container
   static PostgreSQLContainer postgres =
           new PostgreSQLContainer("postgres:17");

   @Test
   void test1() {
       // test talking to postgres
   }

   @Test
   void test2() {
       // test talking to postgres
   }
}
```

By adding the `@Testcontainers` annotation on the class and `@Container` annotation on the container declaration field, JUnit will automatically start the container before running tests and automatically destroy it after the test has been executed.

**NOTE:** When you use the `@Container` annotation on `static` container declarations, like in the example above, only one instance of that container is created for all test cases within a single test class. If you want to create a separate instance for each test case, make it a `non-static` field. Remember that spinning up a new container instance for every test case is not recommended, as it slows down the test execution.

## Integration testing Spring Boot applications using Testcontainers

While Testcontainers can be used with any Java application, frameworks like Spring Boot provide out-of-the-box support for Testcontainers to make integration testing easier.

Let’s explore how you can test Spring Data JPA repositories and Spring Boot REST APIs that use a PostgreSQL database with Testcontainers.

If you want to learn how to build a Spring Boot REST API with Spring Data JPA, PostgreSQL, and Flyway, read the following blog posts:

- How to Build a CRUD REST API Using Spring Boot 

- How to Use Flyway for Database Migrations in Spring Boot Applications

Let’s use the bookmarks Spring Boot application that was created in the above blog posts and write tests for the Spring Data JPA repository and REST API using Testcontainers.

First, clone the `flyway` branch of the bookmarks repository and create a new branch called `testcontainers`. The `flyway` branch uses Flyway database migrations to manage database schema changes.

```
$ git clone --branch flyway https://github.com/sivaprasadreddy/bookmarks.git
$ git checkout -b testcontainers
```

Now, add the following Testcontainers PostgreSQL module and junit-jupiter dependencies:

```
// Gradle build.gradle
testImplementation 'org.testcontainers:postgresql'
testImplementation 'org.testcontainers:junit-jupiter'

//Maven pom.xml
<dependency>
   <groupId>org.testcontainers</groupId>
   <artifactId>postgresql</artifactId>
   <scope>test</scope>
</dependency>
<dependency>
   <groupId>org.testcontainers</groupId>
   <artifactId>junit-jupiter</artifactId>
   <scope>test</scope>
</dependency>
```

Spring Boot provides slice test annotations such as `@DataJpaTest`, `@DataMongoTest`, and `@WebMvcTest` to test a slice of the application. By using these slice test annotations, you can write tests that load only the necessary components of the application. To write integration tests, Spring Boot’s `@SpringBootTest` annotation bootstraps the entire application context, loading all the components.

### Testing Spring Data JPA repositories

Here’s the BookmarkRepository interface within the bookmarks project:

```
package com.jetbrains.bookmarks;

import org.springframework.data.jpa.repository.JpaRepository;

import java.util.List;
import java.util.Optional;

public interface BookmarkRepository extends JpaRepository<Bookmark, Long> {
 List<BookmarkInfo> findAllByOrderByCreatedAtDesc();

 Optional<BookmarkInfo> findBookmarkById(Long id);
}
```

A common mistake when testing repositories is using an in-memory database like H2 for testing while using a different type of database like PostgreSQL or Oracle Database in production. There is no guarantee that the implementation that worked with an in-memory database will work in the same way with other databases. Additionally, the in-memory database may not have all the features provided by your production database and vice versa.

For example, the following is a valid query for PostgreSQL:

```
INSERT INTO items(id, code, name) VALUES(?,?,?) ON CONFLICT DO NOTHING;
```

However, this query doesn’t work with the H2 database by default. Executing the above query with the H2 database results in the following exception:

```
Caused by: org.h2.jdbc.JdbcSQLException: Syntax error in SQL statement "INSERT INTO items (id, code, name) VALUES (?, ?, ?) ON[*] CONFLICT DO NOTHING";"
```

Furthermore, the H2 database supports the `ROWNUM()` function, whereas PostgreSQL does not.

To ensure consistent behavior between testing and production, using the same database type is recommended. Testcontainers is useful because it facilitates this approach.

Now, let’s write tests for the `BookmarkRepository` using the `@DataJpaTest` slice test annotation using Testcontainers:

```
package com.jetbrains.bookmarks;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.test.context.DynamicPropertyRegistry;
import org.springframework.test.context.DynamicPropertySource;
import org.testcontainers.containers.PostgreSQLContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

import static org.assertj.core.api.Assertions.assertThat;

@DataJpaTest
@Testcontainers
class BookmarkRepositoryTest {
   @Container
   static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:17");

   @DynamicPropertySource
   static void configureProperties(DynamicPropertyRegistry registry) {
       registry.add("spring.datasource.url", postgres::getJdbcUrl);
       registry.add("spring.datasource.username", postgres::getUsername);
       registry.add("spring.datasource.password", postgres::getPassword);
   }

   @Autowired
   BookmarkRepository bookmarkRepository;

   @Test
   void shouldFindBookmarkById() {
       var bookmark = new Bookmark("JetBrains Blog","https://blog.jetbrains.com");
       Long bookmarkId = bookmarkRepository.save(bookmark).getId();

       var bookmarkInfo = bookmarkRepository.findBookmarkById(bookmarkId).orElseThrow();
       assertThat(bookmarkInfo.getTitle()).isEqualTo("JetBrains Blog");
       assertThat(bookmarkInfo.getUrl()).isEqualTo("https://blog.jetbrains.com");
   }
}
```

We are using the `@Testcontainers` and `@Container` annotations to start the PostgreSQL container before running tests and destroying it afterward. Once the PostgreSQL container is started, we can configure our Spring Boot application data source properties to connect to the PostgreSQL container using `@DynamicPropertySource` – this allows us to override the default properties.

If you run the test, you can see that a PostgreSQL container starts, and the test executes against the PostgreSQL database container.

Spring Boot 3.1.0 introduced ServiceConnection support, simplifying the usage of Testcontainers for testing and local development. To use this feature, add the following dependency:

```
// Gradle build.gradle
testImplementation 'org.springframework.boot:spring-boot-testcontainers'

//Maven pom.xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-testcontainers</artifactId>
   <scope>test</scope>
</dependency>
```

Now, let’s update the `BookmarkRepositoryTest` to use the ServiceConnection support:

```
import org.springframework.boot.testcontainers.service.connection.ServiceConnection;

@DataJpaTest
@Testcontainers
class BookmarkRepositoryTest {
   
   @Container
   @ServiceConnection
   static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:17");

   @Autowired
   BookmarkRepository bookmarkRepository;

   @Test
   void shouldFindBookmarkById() {
       //...
   }
}
```

By adding the `@ServiceConnection` annotation to the container field definition, Spring Boot will automatically configure the data source properties pointing to the PostgreSQL database container. There is no need to explicitly configure the data source properties using `@DynamicPropertySource`.

If you create a Spring Boot project with PostgreSQL and Testcontainers dependencies, a class with the name `TestcontainersConfiguration` will be created under the `src/test/java` directory:

```
package com.jetbrains.bookmarks;

import org.springframework.boot.test.context.TestConfiguration;
import org.springframework.boot.testcontainers.service.connection.ServiceConnection;
import org.springframework.context.annotation.Bean;
import org.testcontainers.containers.PostgreSQLContainer;
import org.testcontainers.utility.DockerImageName;

@TestConfiguration(proxyBeanMethods = false)
class TestcontainersConfiguration {

   @Bean
   @ServiceConnection
   PostgreSQLContainer<?> postgresContainer() {
      return new PostgreSQLContainer<>(DockerImageName.parse("postgres:17"));
   }
}
```

If you introduce Testcontainers in an existing application, you need to manually create this class.

Instead of defining the `PostgreSQLContainer` in each test class, you can import the `TestcontainersConfiguration` class and use it in your test:

```
@DataJpaTest
@Import(TestcontainersConfiguration.class)
class BookmarkRepositoryTest {

   @Autowired
   BookmarkRepository bookmarkRepository;

   @Test
   void shouldFindBookmarkById() {
       //...
   }
}
```

The `PostgreSQLContainer` bean definition and the `@ServiceConnection` annotation defined in `TestcontainersConfiguration` will take care of spinning up a PostgreSQL database container and configure the application to connect to that database.

### Testing Spring Boot REST APIs

While slice tests are valuable, integration tests with real dependencies, like databases and message brokers, are crucial for verifying overall system behavior.

Similar to how we used Testcontainers to test Spring Data JPA repositories, you can write integration tests with the `@SpringBootTest` annotation and use Testcontainers to spin up the dependencies required by your application.

In the bookmarks application, there is the `GET /api/bookmarks API` endpoint, which returns all the bookmarks ordered from newest to oldest.

Let’s write an integration test to test this endpoint:

```
package com.jetbrains.bookmarks;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.context.annotation.Import;

import static org.assertj.core.api.Assertions.assertThat;
import static org.springframework.boot.test.context.SpringBootTest.WebEnvironment.RANDOM_PORT;

@SpringBootTest(webEnvironment = RANDOM_PORT)
@Import(TestcontainersConfiguration.class)
class BookmarkControllerTest {

   @Autowired
   private TestRestTemplate restTemplate;

   @Autowired
   private BookmarkRepository bookmarkRepository;

   @BeforeEach
   void setUp() {
       bookmarkRepository.deleteAllInBatch();
   }

   @Test
   void shouldGetAllBookmarks() {
       bookmarkRepository.save(new Bookmark("JetBrains Blog","https://blog.jetbrains.com"));
       bookmarkRepository.save(new Bookmark("IntelliJ IDEA Blog","https://blog.jetbrains.com/idea/"));

       Bookmark[] bookmarks = restTemplate.getForObject("/api/bookmarks", Bookmark[].class);

       assertThat(bookmarks.length).isEqualTo(2);
       assertThat(bookmarks[0].getTitle()).isEqualTo("IntelliJ IDEA Blog");
       assertThat(bookmarks[1].getTitle()).isEqualTo("JetBrains Blog");
   }
}
```

We used the `@SpringBootTest` annotation, which loads the entire application context and is configured to start the server on a random available port on the host machine. We also imported the `TestcontainersConfiguration` class so that the required dependencies (in our case, PostgreSQL) are provisioned using Testcontainers. We then autowired `TestRestTemplate` to make calls to our API endpoints. We also deleted all existing bookmarks before running tests using the `setUp()` JUnit callback method, so we have fresh data for each test run.

In the test case, we saved two new bookmarks and fetched them, invoking the `GET /api/bookmarks` endpoint, and asserting the expected results.

You can find the complete code here.

## Conclusion

Testcontainers helps you test applications using real dependencies instead of relying on mock objects or in-memory alternatives, giving you more confidence in your test suite and your application’s behavior. Additionally, Spring Boot’s out-of-the-box support for Testcontainers makes it even easier to write integration tests.

Spring Boot’s `@ServiceConnection` support is available for the most commonly used technologies. Even if there is no direct support provided by Spring Boot, you can use the Testcontainers `GenericContainer` abstraction to work with any containerized dependency. You can find more information about Testcontainers and its features here.

Go to Source
