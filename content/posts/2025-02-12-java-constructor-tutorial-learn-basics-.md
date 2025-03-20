---
title: "Java Constructor Tutorial Learn Basics and Best Practices"
date: 2025-02-12T00:00:00.000Z
draft: false
type: posts
categories: 
- java
---
# Java Constructor Tutorial Learn Basics and Best Practices

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

Constructor in java is used to create the instance of the class. Constructors are almost similar to methods except for two things - its name is the same as the class name and it has no return type. Sometimes constructors are also referred to as special methods to initialize an object.

[Constructor in Java](#constructor-in-java)[](#constructor-in-java)
-------------------------------------------------------------------

Whenever we use `new` keyword to create an instance of a class, the constructor is invoked and the object of the class is returned. Since constructor can only return the object to class, it’s implicitly done by java runtime and we are not supposed to add a return type to it. If we add a return type to a constructor, then it will become a method of the class. This is the way java runtime distinguish between a normal method and a constructor. Let’s assume we have following code in `Employee` class.

```
public Employee() {
 System.out.println("Employee Constructor");
}

public Employee Employee() {
 System.out.println("Employee Method");
 return new Employee();
}
```

Here the first one is a constructor, notice that there is no return type and no return statement. The second one is a normal method where we are again calling the first constructor to get Employee instance and return it. It’s recommended to not have method name same as the class name because it creates confusion.

[Types of Constructor in Java](#types-of-constructor-in-java)[](#types-of-constructor-in-java)
----------------------------------------------------------------------------------------------

There are three types of constructor in java.

1.  Default Constructor
2.  No-Args constructor
3.  Parameterized constructor

Let’s look into all these constructor types with example programs.

### [Default Constructor in Java](#default-constructor-in-java)[](#default-constructor-in-java)

It’s not required to always provide a constructor implementation in the class code. If we don’t provide a constructor, then java provides default constructor implementation for us to use. Let’s look at a simple program where default constructor is being used since we will not explicitly define a constructor.

```
package com.journaldev.constructor;

public class Data {

 public static void main(String[] args) {
  Data d = new Data();
 }
}
```

1.  Default constructor only role is to initialize the object and return it to the calling code.
2.  Default constructor is always without argument and provided by java compiler only when there is no existing constructor defined.
3.  Most of the time we are fine with default constructor itself as other properties can be accessed and initialized through getter setter methods.

### [No-Args Constructor](#no-args-constructor)[](#no-args-constructor)

Constructor without any argument is called a no-args constructor. It’s like overriding the default constructor and used to do some pre-initialization stuff such as checking resources, network connections, logging, etc. Let’s have a quick look at the no-args constructor in java.

```
package com.journaldev.constructor;

public class Data {
        //no-args constructor
 public Data() {
  System.out.println("No-Args Constructor");
 }
 public static void main(String[] args) {
  Data d = new Data();
 }
}
```

Now when we will call `new Data()`, then our no-args constructor will be called. Below image illustrates this behavior, check the console output of the program. ![no args constructor in java](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/02/no-args-constructor-in-java.png)

### [Parameterized Constructor](#parameterized-constructor)[](#parameterized-constructor)

Constructor with arguments is called parameterized constructor. Let’s look at the example of parameterized constructor in java.

```
package com.journaldev.constructor;

public class Data {

 private String name;

 public Data(String n) {
  System.out.println("Parameterized Constructor");
  this.name = n;
 }

 public String getName() {
  return name;
 }

 public static void main(String[] args) {
  Data d = new Data("Java");
  System.out.println(d.getName());
 }

}
```

![java class constructor with parameters](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/02/parameterized-constructor-in-java.png)

[Constructor Overloading in Java](#constructor-overloading-in-java)[](#constructor-overloading-in-java)
-------------------------------------------------------------------------------------------------------

When we have more than one constructors, then it’s constructor overloading in java. Let’s look at an example of constructor overloading in java program.

```
package com.journaldev.constructor;

public class Data {

 private String name;
 private int id;

 //no-args constructor
 public Data() {
  this.name = "Default Name";
 }
 //one parameter constructor
 public Data(String n) {
  this.name = n;
 }
 //two parameter constructor
 public Data(String n, int i) {
  this.name = n;
  this.id = i;
 }

 public String getName() {
  return name;
 }

 public int getId() {
  return id;
 }

 @Override
 public String toString() {
  return "ID="+id+", Name="+name;
 }
 public static void main(String[] args) {
  Data d = new Data();
  System.out.println(d);
  
  d = new Data("Java");
  System.out.println(d);
  
  d = new Data("Pankaj", 25);
  System.out.println(d);
  
 }

}
```

[Private Constructor in Java](#private-constructor-in-java)[](#private-constructor-in-java)
-------------------------------------------------------------------------------------------

Note that we can’t use [abstract](/community/tutorials/abstract-class-in-java), final, [static](/community/tutorials/static-keyword-in-java) and synchronized keywords with constructors. However we can use access modifiers to control the instantiation of class object. Using `public` and `default` access is still fine, but what is the use of making a constructor private? In that case any other class won’t be able to create the instance of the class. Well, a constructor is made private in case we want to implement [singleton design pattern](/community/tutorials/java-singleton-design-pattern-best-practices-examples). Since java automatically provides default constructor, we have to explicitly create a constructor and keep it private. Client classes are provided with a utility static method to get the instance of the class. An example of private constructor for `Data` class is given below.

```
// private constructor
private Data() {
 //empty constructor for singleton pattern implementation
 //can have code to be used inside the getInstance() method of class
}
```

[Constructor Chaining in Java](#constructor-chaining-in-java)[](#constructor-chaining-in-java)
----------------------------------------------------------------------------------------------

When a constructor calls another constructor of the same class, it’s called constructor chaining. We have to use `this` keyword to call another constructor of the class. Sometimes it’s used to set some default values of the class variables. Note that another constructor call should be the first statement in the code block. Also, there should not be recursive calls that will create an infinite loop. Let’s see an example of constructor chaining in java program.

```
package com.journaldev.constructor;

public class Employee {

 private int id;
 private String name;
 
 public Employee() {
  this("John Doe", 999);
  System.out.println("Default Employee Created");
 }
 
 public Employee(int i) {
  this("John Doe", i);
  System.out.println("Employee Created with Default Name");
 }
 public Employee(String s, int i) {
  this.id = i;
  this.name = s;
  System.out.println("Employee Created");
 }
 public static void main(String[] args) {

  Employee emp = new Employee();
  System.out.println(emp);
  Employee emp1 = new Employee(10);
  System.out.println(emp1);
  Employee emp2 = new Employee("Pankaj", 20);
  System.out.println(emp2);
 }

 @Override
 public String toString() {
  return "ID = "+id+", Name = "+name;
 }
 public int getId() {
  return id;
 }

 public void setId(int id) {
  this.id = id;
 }

 public String getName() {
  return name;
 }

 public void setName(String name) {
  this.name = name;
 }

}
```

I have overridden the [toString()](/community/tutorials/java-long-to-string) method to print some useful information about Employee object. Below is the output produced by above program.

```
Employee Created
Default Employee Created
ID = 999, Name = John Doe
Employee Created
Employee Created with Default Name
ID = 10, Name = John Doe
Employee Created
ID = 20, Name = Pankaj
```

Notice how one constructor is being called from another constructor, that’s called constructor chaining process.

[Java Super Constructor](#java-super-constructor)[](#java-super-constructor)
----------------------------------------------------------------------------

Sometimes a class is inherited from a superclass, in that case, if we have to call superclass constructor then we can do it using `super` keyword. Let’s have a look at an example of using super class constructor. Note that super constructor call should be the first statement in the child class constructor. Also when instantiating child class constructor, java first initializes the super class and then child class. So if the super class constructor is not explicitly called then default or no-args constructor is called by java runtime. Let’s understand these concepts through some example program. Let’s assume we have two classes like below.

```
package com.journaldev.constructor;

public class Person {

 private int age;

 public Person() {
  System.out.println("Person Created");
 }

 public Person(int i) {
  this.age = i;
  System.out.println("Person Created with Age = " + i);
 }

}
```

```
package com.journaldev.constructor;

public class Student extends Person {

 private String name;

 public Student() {
  System.out.println("Student Created");
 }

 public Student(int i, String n) {
  super(i); // super class constructor called
  this.name = n;
  System.out.println("Student Created with name = " + n);
 }

}
```

Now if we create a Student object like below;

```
Student st = new Student();
```

What will be the output produced? The output of the above code will be:

```
Person Created
Student Created
```

So the call went to the no-args constructor of Student class since there was no super call in the first statement the no-args or default constructor of Person class is called. Hence the output. What if we are using parameterized constructor of Student class as `Student st = new Student(34, "Pankaj");`, the output will be:

```
Person Created with Age = 34
Student Created with name = Pankaj
```

Here the output is clear because we are explicitly calling superclass constructor, so Java doesn’t need to do any extra work from their side.

[Java Copy Constructor](#java-copy-constructor)[](#java-copy-constructor)
-------------------------------------------------------------------------

Java copy constructor takes the object of the same class as an argument and creates a copy of it. Sometimes we need a copy of another object to do some processing. We can do this by following ways:

1.  implement [cloning](/community/tutorials/java-clone-object-cloning-java)
2.  providing a utility method for [deep copy](/community/tutorials/java-string-copy) of the object.
3.  Having a copy constructor

Now let’s see how to write a copy constructor. Suppose we have a class `Fruits` like below.

```
package com.journaldev.constructor;

import java.util.ArrayList;
import java.util.List;

public class Fruits {

 private List<String> fruitsList;

 public List<String> getFruitsList() {
  return fruitsList;
 }

 public void setFruitsList(List<String> fruitsList) {
  this.fruitsList = fruitsList;
 }

 public Fruits(List<String> fl) {
  this.fruitsList = fl;
 }
 
 public Fruits(Fruits fr) {
  List<String> fl = new ArrayList<>();
  for (String f : fr.getFruitsList()) {
   fl.add(f);
  }
  this.fruitsList = fl;
 }
}
```

Notice that `Fruits(Fruits fr)` is performing a deep copy to return the copy of the object. Let’s look at a test program to understand why it’s better to have copy constructor to copy an object.

```
package com.journaldev.constructor;

import java.util.ArrayList;
import java.util.List;

public class CopyConstructorTest {

 public static void main(String[] args) {
  List<String> fl = new ArrayList<>();
  fl.add("Mango");
  fl.add("Orange");

  Fruits fr = new Fruits(fl);

  System.out.println(fr.getFruitsList());

  Fruits frCopy = fr;
  frCopy.getFruitsList().add("Apple");

  System.out.println(fr.getFruitsList());

  frCopy = new Fruits(fr);
  frCopy.getFruitsList().add("Banana");
  System.out.println(fr.getFruitsList());
  System.out.println(frCopy.getFruitsList());

 }

}
```

The output of the above program is:

```
[Mango, Orange]
[Mango, Orange, Apple]
[Mango, Orange, Apple]
[Mango, Orange, Apple, Banana]
```

Notice that when copy constructor is used, both the original object and its copy are unrelated to each other and any modifications in one of them will not reflect into other. That’s all for the constructor in java.

[Advanced Use Cases of Constructors in Frameworks](#advanced-use-cases-of-constructors-in-frameworks)[](#advanced-use-cases-of-constructors-in-frameworks)
----------------------------------------------------------------------------------------------------------------------------------------------------------

### [Constructors in Spring and Hibernate](#constructors-in-spring-and-hibernate)[](#constructors-in-spring-and-hibernate)

Constructors play a crucial role in Java frameworks such as **[Spring](/community/tutorials/spring-framework)** and **[Hibernate](/community/tutorials/hibernate-tutorial)**:

1.  **Spring Framework**: Uses **constructor injection** to enforce dependency management, particularly for **immutable beans**.
    
    ```
       // This is a Spring component class for UserService.
       // It is used to manage the user data and operations.
       @Component
       public class UserService {
          // This is a final field for UserRepository.
          // It is used to access the user data.
          private final UserRepository userRepository;
    
          // This is a constructor for UserService.
          // It is used to initialize the UserRepository.
          @Autowired
          public UserService(UserRepository userRepository) {
             this.userRepository = userRepository;
          }
       }
    ```
    
2.  **Hibernate ORM**: Hibernate ORM uses default (no-argument) constructors to instantiate objects when retrieving data from the database. The default constructor is essential because Hibernate creates objects using reflection and needs a way to instantiate an entity before setting its properties.
    
    ```
    
    import jakarta.persistence.*;
    
    @Entity
    @Table(name = "users")
    public class User {
       
       @Id
       @GeneratedValue(strategy = GenerationType.IDENTITY)
       private Long id;
       
       private String name;
       private String email;
    
       // **Default Constructor (Required by Hibernate)**
       public User() {
       }
    
       // **Parameterized Constructor (For Custom Initialization)**
       public User(String name, String email) {
          this.name = name;
          this.email = email;
       }
    
       // Getters and Setters
       public Long getId() {
          return id;
       }
    
       public void setId(Long id) {
          this.id = id;
       }
    
       public String getName() {
          return name;
       }
    
       public void setName(String name) {
          this.name = name;
       }
    
       public String getEmail() {
          return email;
       }
    
       public void setEmail(String email) {
          this.email = email;
       }
    
    }
    ```
    

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. What is a constructor in Java?](#1-what-is-a-constructor-in-java)[](#1-what-is-a-constructor-in-java)

A constructor in Java is a special method that initializes objects. It is automatically called when an object is instantiated. Unlike regular methods, constructors do not have a return type, not even `void`. Constructors help in setting up initial values for object properties and ensure that an object is in a valid state from the beginning.

### [2\. What are the four types of constructors in Java?](#2-what-are-the-four-types-of-constructors-in-java)[](#2-what-are-the-four-types-of-constructors-in-java)

Java supports different types of constructors, which include:

-   **Default Constructor** – A no-argument constructor that initializes instance variables with default values.
-   **Parameterized Constructor** – Allows passing arguments to set initial values for object properties.
-   **Copy Constructor** – Creates a new object by copying an existing object’s values.
-   **Private Constructor** – Restricts object instantiation from outside the class, commonly used in the Singleton pattern. You can refer to this tutorial on [Builder Design Pattern in Java](/community/tutorials/builder-design-pattern-in-java).

### [3\. Why use a constructor instead of a method?](#3-why-use-a-constructor-instead-of-a-method)[](#3-why-use-a-constructor-instead-of-a-method)

Constructors and methods differ in purpose. A constructor is called automatically when an object is created, ensuring proper initialization. Methods, on the other hand, must be explicitly invoked and are used to define behavior. Additionally, a constructor cannot return a value, whereas methods can return results based on business logic.

**Feature**

**Constructors**

**Methods**

**Purpose**

Initialize objects

Define behavior

**Invocation**

Automatically called

Explicitly invoked

**Return Type**

No return type

Can return a value

**Overloading**

Yes

Yes

**Overriding**

No

Yes

**Access Modifiers**

Yes

Yes

**Static**

No

Yes

**Final**

No

Yes

**Abstract**

No

Yes

**Synchronized**

No

Yes

### [4\. What is the difference between a constructor and an object in Java?](#4-what-is-the-difference-between-a-constructor-and-an-object-in-java)[](#4-what-is-the-difference-between-a-constructor-and-an-object-in-java)

-   A **constructor** is a block of code within a class that initializes an object.
-   An **object** is an instance of a class created using the `new` keyword.
-   The constructor is **executed only once** per object creation, while objects persist in memory and can be used multiple times.

**Feature**

**Constructors**

**Objects**

**Purpose**

Initializes objects

Represents an instance of a class

**Creation**

Executed once per object creation

Created using the `new` keyword

**Duration**

Executes once

Persists in memory and can be used multiple times

**Role**

Sets initial state of an object

Represents a real-world entity or concept

### [5\. What is the benefit of using a constructor in Java?](#5-what-is-the-benefit-of-using-a-constructor-in-java)[](#5-what-is-the-benefit-of-using-a-constructor-in-java)

Constructors provide multiple benefits, including:

-   **Automatic Object Initialization**: Ensures an object starts in a valid state.
-   **Code Reusability**: Reduces redundancy by centralizing initialization logic.
-   **Encapsulation & Immutability**: Used in frameworks like Spring and Hibernate to enforce dependency injection and configuration settings. You can refer to this tutorial on [Java Reflection Example](/community/tutorials/java-reflection-example-tutorial).

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

Constructors are an essential part of Java programming, playing a crucial role in object initialization and class design. From simple default constructors to more advanced constructor chaining and dependency injection in frameworks like Spring and Hibernate, understanding how constructors work can help developers build robust, maintainable applications.

By leveraging constructors effectively, you can enforce immutability, promote reusability, and optimize performance.

You can check out these related tutorials to learn more:

-   [Builder Design Pattern in Java](/community/tutorials/builder-design-pattern-in-java)
-   [Java Reflection Example Tutorial](/community/tutorials/java-reflection-example-tutorial)
-   [Scanner Class in Java](/community/tutorials/scanner-class-in-java)

#### [Source](https://www.digitalocean.com/community/tutorials/constructor-in-java)

<br/>
---
