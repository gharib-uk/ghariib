---
title: "Multiple Inheritance in Java Explained with Examples and Best Practices"
date: 2025-02-14T00:00:00.000Z
draft: false
type: posts
categories: 
- java
---
# Multiple Inheritance in Java Explained with Examples and Best Practices

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

Inheritance is one of the fundamental principles of Object-Oriented Programming (OOP) that allows one class (the child class or subclass) to inherit fields and methods from another class (the parent class or superclass). This promotes code reuse, modularity, and better maintainability.

In this article, we will deep-dive into the concept of multiple inheritance in Java, building upon previous tutorials on **[inheritance](/community/tutorials/inheritance-java-example)**, **[interface](/community/tutorials/interface-in-java)**, and **[composition](/community/tutorials/composition-vs-inheritance)** in Java.

[How to Implement Inheritance in Java](#how-to-implement-inheritance-in-java)[](#how-to-implement-inheritance-in-java)
----------------------------------------------------------------------------------------------------------------------

Inheritance in Java is implemented using the `extends` keyword. Here’s an example:

```
// Parent class
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

// Child class inheriting from Animal
class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.makeSound(); // Inherited method
        dog.bark(); // Child class method
    }
}
```

This example demonstrates single inheritance, where the `Dog` class inherits behavior from the `Animal` class.

[Different Types of Inheritance in Java](#different-types-of-inheritance-in-java)[](#different-types-of-inheritance-in-java)
----------------------------------------------------------------------------------------------------------------------------

Java supports different types of inheritance, which define the relationships between classes. These include:

1.  **Single Inheritance**: A subclass inherits from a single parent class.
    
    For example:
    
    ```
    // Parent class
    class Animal {
       void makeSound() {
          System.out.println("Animal makes a sound");
       }
    }
    
    // Child class inheriting from Animal
    class Dog extends Animal {
       void bark() {
          System.out.println("Dog barks");
       }
    }
    ```
    
2.  **Multilevel Inheritance**: A subclass derives from another subclass, forming a hierarchy.
    
    For example:
    
    ```
    // Grandparent class
    class Animal {
       void makeSound() {
          System.out.println("Animal makes a sound");
       }
    }
    
    // Parent class inheriting from Animal
    class Mammal extends Animal {
       void eat() {
          System.out.println("Mammal eats");
       }
    }
    
    // Child class inheriting from Mammal
    class Dog extends Mammal {
       void bark() {
          System.out.println("Dog barks");
       }
    }
    ```
    
3.  **Hierarchical Inheritance**: Multiple classes inherit from the same parent class.
    
    For example:
    
    ```
    // Parent class
    class Animal {
       void makeSound() {
          System.out.println("Animal makes a sound");
       }
    }
    
    // Child class 1 inheriting from Animal
    class Dog extends Animal {
       void bark() {
          System.out.println("Dog barks");
       }
    }
    
    // Child class 2 inheriting from Animal
    class Cat extends Animal {
       void meow() {
          System.out.println("Cat meows");
       }
    }
    ```
    
4.  **Hybrid Inheritance**: A mix of two or more types of inheritance. Java does not support direct hybrid inheritance but can be achieved using [interfaces](/community/tutorials/interface-in-java). Here’s an example:
    
    ```
    // Interface 1
    interface Flyable {
       void fly();
    }
    
    // Interface 2
    interface Walkable {
       void walk();
    }
    
    // Parent class
    class Animal {
       void makeSound() {
          System.out.println("Animal makes a sound");
       }
    }
    
    // Child class inheriting from Animal and implementing Flyable and Walkable
    class Bird extends Animal implements Flyable, Walkable {
       @Override
       public void fly() {
          System.out.println("Bird flies");
       }
    
       @Override
       public void walk() {
          System.out.println("Bird walks");
       }
    }
    ```
    

For a deeper dive into OOP concepts in Java, check this tutorial on [OOPS Concepts in Java - OOPS Concepts Example](/community/tutorials/oops-concepts-java-example).

[Performance Considerations of Inheritance in Java](#performance-considerations-of-inheritance-in-java)[](#performance-considerations-of-inheritance-in-java)
-------------------------------------------------------------------------------------------------------------------------------------------------------------

While inheritance promotes code reuse, it can impact memory usage and performance if not used wisely. Key considerations include:

-   **Memory Consumption**: Each subclass instance contains data from both the subclass and superclass, leading to increased memory consumption.
    
-   **Method Resolution**: The JVM must resolve method calls dynamically, which might introduce slight overhead in method lookup.
    
-   **Deep Inheritance Trees**: Excessive inheritance levels can lead to a complex class hierarchy, making debugging and performance tuning difficult.
    

For performance-critical applications, consider alternatives like composition, which often provides better flexibility and maintainability.

[FAQ](#faq)[](#faq)
-------------------

### [1\. What is inheritance in Java?](#1-what-is-inheritance-in-java)[](#1-what-is-inheritance-in-java)

Inheritance in Java is a mechanism where a subclass derives properties and behaviors from a parent class, allowing for code reuse and hierarchical structuring. You can read more about inheritance in this tutorial on [Inheritance in Java](/community/tutorials/inheritance-java-example).

### [2\. What are the types of inheritance in Java?](#2-what-are-the-types-of-inheritance-in-java)[](#2-what-are-the-types-of-inheritance-in-java)

Java supports single, multilevel, hierarchical, and hybrid inheritance. However, multiple inheritance is not supported directly due to the diamond problem.

### [3\. How does the `extends` keyword work in Java?](#3-how-does-the-extends-keyword-work-in-java)[](#3-how-does-the-extends-keyword-work-in-java)

The `extends` keyword is used to indicate that a class is inheriting from another class. The child class gains access to the parent class’s methods and fields. Here’s an example:

```
// Parent class
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

// Child class inheriting from Animal
class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}
```

### [4\. What is the difference between inheritance and composition in Java?](#4-what-is-the-difference-between-inheritance-and-composition-in-java)[](#4-what-is-the-difference-between-inheritance-and-composition-in-java)

Inheritance defines a “is-a” relationship, while composition represents a “has-a” relationship. Composition is often preferred for better flexibility. Read more about the differences in this tutorial on [Composition vs Inheritance](/community/tutorials/composition-vs-inheritance).

**Concept**

**Inheritance**

**Composition**

Relationship

“is-a”

“has-a”

Flexibility

Less flexible

More flexible

Code Reuse

Promotes code reuse

Promotes code reuse

Complexity

Can lead to tight coupling

Encourages loose coupling

### [5\. Can a subclass override a method in Java?](#5-can-a-subclass-override-a-method-in-java)[](#5-can-a-subclass-override-a-method-in-java)

Yes, a subclass can override a method from the parent class using the `@Override` annotation. This allows the subclass to provide a specific implementation of the inherited method.

Here’s an example:

```
// Parent class
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

// Child class inheriting from Animal
class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Dog makes a sound");
    }
}
```

### [6\. Why does Java not support multiple inheritance?](#6-why-does-java-not-support-multiple-inheritance)[](#6-why-does-java-not-support-multiple-inheritance)

Java does not support [multiple inheritance](/community/tutorials/multiple-inheritance-in-java#multiple-inheritance-in-java) for classes to prevent ambiguity and [diamond problems](/community/tutorials/multiple-inheritance-in-java#diamond-problem-in-java). This decision was made to ensure that the language remains simple and easy to use, avoiding the complexities that can arise from multiple inheritance. However, multiple inheritance can be achieved using [interfaces](/community/tutorials/interface-in-java), which provide a way to implement multiple behaviors without the risks associated with [multiple inheritance](/community/tutorials/multiple-inheritance-in-java#multiple-inheritance-in-java).

### [7\. When should I avoid using inheritance in Java?](#7-when-should-i-avoid-using-inheritance-in-java)[](#7-when-should-i-avoid-using-inheritance-in-java)

Avoid inheritance when:

-   A deep inheritance hierarchy complicates maintenance.
-   Code reuse can be better achieved with composition.
-   There is no clear is-a relationship between classes.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

Understanding inheritance and its best practices ensures that your Java applications remain efficient, modular, and easy to maintain. By balancing inheritance and composition, you can achieve a well-structured application.

You can learn more about object-oriented programming in this tutorial on [OOPS concept in Java](/community/tutorials/oops-concepts-java-example).

#### [Source](https://www.digitalocean.com/community/tutorials/multiple-inheritance-in-java)

<br/>
---
