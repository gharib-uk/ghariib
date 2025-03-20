---
title: "Java Generics Explained Benefits Examples and Best Practice"
date: 2025-02-10T00:00:00.000Z
draft: false
type: posts
categories: 
- java
---
# Java Generics Explained Benefits Examples and Best Practice

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

**Java Genrics** is one of the most important features introduced in Java 5. If you have been working on [Java Collections](/community/tutorials/collections-in-java-tutorial) and with version 5 or higher, I am sure that you have used it. **Generics in Java** with collection classes is very easy but provides many more features than just creating the type of collection. We will try to learn the features of generics in this article.

[Generics in Java](#generics-in-java)[](#generics-in-java)
----------------------------------------------------------

Generics was added in Java 5 to provide **compile-time type checking** and removing risk of `ClassCastException` that was common while working with collection classes. The whole collection framework was re-written to use generics for type-safety. Let’s see how generics help us using collection classes safely.

```
List list = new ArrayList();
list.add("abc");
list.add(new Integer(5)); //OK

for(Object obj : list){
 //type casting leading to ClassCastException at runtime
    String str=(String) obj; 
}
```

Above code compiles fine but throws ClassCastException at runtime because we are trying to cast Object in the list to String whereas one of the element is of type Integer. After Java 5, we use collection classes like below.

```
List<String> list1 = new ArrayList<String>(); // java 7 ? List<String> list1 = new ArrayList<>(); 
list1.add("abc");
//list1.add(new Integer(5)); //compiler error

for(String str : list1){
     //no type casting needed, avoids ClassCastException
}
```

Notice that at the time of list creation, we have specified that the type of elements in the list will be String. So if we try to add any other type of object in the list, the program will throw compile-time error. Also notice that in for loop, we don’t need typecasting of the element in the list, hence removing the ClassCastException at runtime.

### [Java Generic Class](#java-generic-class)[](#java-generic-class)

We can define our own classes with generics type. A generic type is a class or interface that is parameterized over types. We use angle brackets (<>) to specify the type parameter. To understand the benefit, let’s say we have a simple class as:

```
package com.journaldev.generics;

public class GenericsTypeOld {

 private Object t;

 public Object get() {
  return t;
 }

 public void set(Object t) {
  this.t = t;
 }

        public static void main(String args[]){
  GenericsTypeOld type = new GenericsTypeOld();
  type.set("Pankaj"); 
  String str = (String) type.get(); //type casting, error prone and can cause ClassCastException
 }
}
```

Notice that while using this class, we have to use type casting and it can produce ClassCastException at runtime. Now we will use java generic class to rewrite the same class as shown below.

```
package com.journaldev.generics;

public class GenericsType<T> {

 private T t;
 
 public T get(){
  return this.t;
 }
 
 public void set(T t1){
  this.t=t1;
 }
 
 public static void main(String args[]){
  GenericsType<String> type = new GenericsType<>();
  type.set("Pankaj"); //valid
  
  GenericsType type1 = new GenericsType(); //raw type
  type1.set("Pankaj"); //valid
  type1.set(10); //valid and autoboxing support
 }
}
```

Notice the use of `GenericsType` class in the main method. We don’t need to do type-casting, and we can remove `ClassCastException` error at runtime. If we don’t provide the type at the creation time, the compiler will warn that “GenericsType is a raw type. References to generic type `GenericsType<T>` should be parameterized”. When we don’t provide the type, the type becomes `Object`, and hence, it allows both String and Integer objects. But, we should always try to avoid this because we will have to use type casting while working on the raw type, which can produce runtime errors.

**Tip**: We can use `@SuppressWarnings("rawtypes")` annotation to suppress the compiler warning, check out **[java annotations tutorial](/community/tutorials/java-annotations)**.

Also, please note that it supports [java autoboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html).

### [Java Generic Interface](#java-generic-interface)[](#java-generic-interface)

Comparable interface is a great example of Generics in interfaces and it’s written as:

```
package java.lang;
import java.util.*;

public interface Comparable<T> {
    public int compareTo(T o);
}
```

In similar way, we can create generic interfaces in java. We can also have multiple type parameters as in Map interface. Again we can provide parameterized value to a parameterized type also, for example `new HashMap<String, List<String>>();` is valid.

### [Java Generic Type](#java-generic-type)[](#java-generic-type)

Java Generic Type Naming convention helps us understanding code easily and having a naming convention is one of the best practices of Java programming language. So generics also comes with its own naming conventions. Usually, type parameter names are single, uppercase letters to make it easily distinguishable from java variables. The most commonly used type parameter names are:

-   E - Element (used extensively by the Java Collections Framework, for example ArrayList, Set etc.)
-   K - Key (Used in Map)
-   N - Number
-   T - Type
-   V - Value (Used in Map)
-   S,U,V etc. - 2nd, 3rd, 4th types

### [Java Generic Method](#java-generic-method)[](#java-generic-method)

Sometimes we don’t want the whole class to be parameterized, in that case, we can create java generics method. Since the [constructor](/community/tutorials/constructor-in-java) is a special kind of method, we can use generics type in constructors too. Here is a class showing an example of a java generic method.

```
package com.journaldev.generics;

public class GenericsMethods {

 //Java Generic Method
 public static <T> boolean isEqual(GenericsType<T> g1, GenericsType<T> g2){
  return g1.get().equals(g2.get());
 }
 
 public static void main(String args[]){
  GenericsType<String> g1 = new GenericsType<>();
  g1.set("Pankaj");
  
  GenericsType<String> g2 = new GenericsType<>();
  g2.set("Pankaj");
  
  boolean isEqual = GenericsMethods.<String>isEqual(g1, g2);
  //above statement can be written simply as
  isEqual = GenericsMethods.isEqual(g1, g2);
  //This feature, known as type inference, allows you to invoke a generic method as an ordinary method, without specifying a type between angle brackets.
  //Compiler will infer the type that is needed
 }
}
```

Notice the _isEqual_ method signature showing syntax to use generics type in methods. Also, notice how to use these methods in our java program. We can specify type while calling these methods or we can invoke them like a normal method. Java compiler is smart enough to determine the type of variable to be used, this facility is called **type inference**.

### [Java Generics Bounded Type Parameters](#java-generics-bounded-type-parameters)[](#java-generics-bounded-type-parameters)

Suppose we want to restrict the type of objects that can be used in the parameterized type, for example in a method that compares two objects and we want to make sure that the accepted objects are Comparables. To declare a bounded type parameter, list the type parameter’s name, followed by the extends keyword, followed by its upper bound, similar like below method.

```
public static <T extends Comparable<T>> int compare(T t1, T t2){
  return t1.compareTo(t2);
 }
```

The invocation of these methods is similar to unbounded method except that if we will try to use any class that is not Comparable, it will throw compile-time error. Bounded type parameters can be used with methods as well as classes and interfaces. Java Generics supports multiple bounds also, i.e <T extends A & B & C>. In this case, A can be an interface or class. If A is class then B and C should be an interface. We can’t have more than one class in multiple bounds.

### [Java Generics and Inheritance](#java-generics-and-inheritance)[](#java-generics-and-inheritance)

We know that [Java inheritance](/community/tutorials/inheritance-java-example) allows us to assign a variable A to another variable B if A is subclass of B. So we might think that any generic type of A can be assigned to generic type of B, but it’s not the case. Let’s see this with a simple program.

```
package com.journaldev.generics;

public class GenericsInheritance {

 public static void main(String[] args) {
  String str = "abc";
  Object obj = new Object();
  obj=str; // works because String is-a Object, inheritance in java
  
  MyClass<String> myClass1 = new MyClass<String>();
  MyClass<Object> myClass2 = new MyClass<Object>();
  //myClass2=myClass1; // compilation error since MyClass<String> is not a MyClass<Object>
  obj = myClass1; // MyClass<T> parent is Object
 }
 
 public static class MyClass<T>{}

}
```

We are not allowed to assign `MyClass<String>` variable to `MyClass<Object>` variable because they are not related, in fact `MyClass<T>` parent is Object.

### [Java Generic Classes and Subtyping](#java-generic-classes-and-subtyping)[](#java-generic-classes-and-subtyping)

We can subtype a generic class or interface by extending or implementing it. The relationship between the type parameters of one class or interface and the type parameters of another are determined by the extends and implements clauses. For example, `ArrayList<E>` `implements List<E>` that `extends Collection<E>`, so `ArrayList<String>` is a subtype of `List<String>` and `List<String>` is subtype of `Collection<String>`. The subtyping relationship is preserved as long as we don’t change the type argument, below shows an example of multiple type parameters.

```
interface MyList<E,T> extends List<E>{
}
```

The subtypes of `List<String>` can be `MyList<String,Object>`,`MyList<String,Integer>` and so on.

### [Java Generics Wildcards](#java-generics-wildcards)[](#java-generics-wildcards)

Question mark (`?`) is the wildcard in generics and represent an unknown type. The wildcard can be used as the type of a parameter, field, or local variable and sometimes as a return type. We can’t use wildcards while invoking a generic method or instantiating a generic class. In the following sections, we will learn about upper bounded wildcards, lower bounded wildcards, and wildcard capture.

#### Java Generics Upper Bounded Wildcard

Upper bounded wildcards are used to relax the restriction on the type of variable in a method. Suppose we want to write a method that will return the sum of numbers in the list, so our implementation will be something like this.

```
public static double sum(List<Number> list){
  double sum = 0;
  for(Number n : list){
   sum += n.doubleValue();
  }
  return sum;
 }
```

Now the problem with above implementation is that it won’t work with List of Integers or Doubles because we know that `List<Integer>` and `List<Double>` are not related, this is when an upper bounded wildcard is helpful. We use generics wildcard with **extends** keyword and the **upper bound** class or interface that will allow us to pass argument of upper bound or it’s subclasses types. The above implementation can be modified like the below program.

```
package com.journaldev.generics;

import java.util.ArrayList;
import java.util.List;

public class GenericsWildcards {

 public static void main(String[] args) {
  List<Integer> ints = new ArrayList<>();
  ints.add(3); ints.add(5); ints.add(10);
  double sum = sum(ints);
  System.out.println("Sum of ints="+sum);
 }

 public static double sum(List<? extends Number> list){
  double sum = 0;
  for(Number n : list){
   sum += n.doubleValue();
  }
  return sum;
 }
}
```

It’s similar like writing our code in terms of interface, in the above method we can use all the methods of upper bound class Number. Note that with upper bounded list, we are not allowed to add any object to the list except null. If we will try to add an element to the list inside the sum method, the program won’t compile.

#### Java Generics Unbounded Wildcard

Sometimes we have a situation where we want our generic method to be working with all types, in this case, an unbounded wildcard can be used. Its same as using `<? extends Object>`.

```
public static void printData(List<?> list){
  for(Object obj : list){
   System.out.print(obj + "::");
  }
 }
```

We can provide `List<String>` or `List<Integer>` or any other type of Object list argument to the _printData_ method. Similar to upper bound list, we are not allowed to add anything to the list.

#### Java Generics Lower bounded Wildcard

Suppose we want to add Integers to a list of integers in a method, we can keep the argument type as `List<Integer>` but it will be tied up with Integers whereas `List<Number>` and `List<Object>` can also hold integers, so we can use a lower bound wildcard to achieve this. We use generics wildcard (?) with **super** keyword and lower bound class to achieve this. We can pass lower bound or any supertype of lower bound as an argument, in this case, java compiler allows to add lower bound object types to the list.

```
public static void addIntegers(List<? super Integer> list){
  list.add(new Integer(50));
 }
```

### [Subtyping using Generics Wildcard](#subtyping-using-generics-wildcard)[](#subtyping-using-generics-wildcard)

```
List<? extends Integer> intList = new ArrayList<>();
List<? extends Number>  numList = intList;  // OK. List<? extends Integer> is a subtype of List<? extends Number>
```

### [Java Generics Type Erasure](#java-generics-type-erasure)[](#java-generics-type-erasure)

Generics in Java was added to provide type-checking at compile time and it has no use at run time, so java compiler uses **type erasure** feature to remove all the generics type checking code in byte code and insert type-casting if necessary. Type erasure ensures that no new classes are created for parameterized types; consequently, generics incur no runtime overhead. For example, if we have a generic class like below;

```
public class Test<T extends Comparable<T>> {

    private T data;
    private Test<T> next;

    public Test(T d, Test<T> n) {
        this.data = d;
        this.next = n;
    }

    public T getData() { return this.data; }
}
```

The Java compiler replaces the bounded type parameter `T` with the first bound interface, Comparable, as below code:

```
public class Test {

    private Comparable data;
    private Test next;

    public Node(Comparable d, Test n) {
        this.data = d;
        this.next = n;
    }

    public Comparable getData() { return data; }
}
```

[Common Mistakes with Generics](#common-mistakes-with-generics)[](#common-mistakes-with-generics)
-------------------------------------------------------------------------------------------------

Even experienced developers make mistakes while using generics. Here are some common pitfalls:

### [1\. Using Raw Types](#1-using-raw-types)[](#1-using-raw-types)

Using raw types defeats the purpose of generics.

```
List list = new ArrayList(); // Avoid this
list.add("Hello");
list.add(123); // No type safety
```

Instead,use parameterized types instead:

```
List<String> list = new ArrayList<>();
```

### [2\. Mixing Generics with Legacy Code](#2-mixing-generics-with-legacy-code)[](#2-mixing-generics-with-legacy-code)

Avoid mixing generic and non-generic code as it can lead to unexpected issues.

### [3\. Incorrect Use of Wildcards](#3-incorrect-use-of-wildcards)[](#3-incorrect-use-of-wildcards)

Example:

```
public void addItem(List<? extends Number> list) {
    // list.add(10); // Compilation error
}
```

Instead, use ? super `T` if modification is required:

```
public void addItem(List<? super Integer> list) {
    list.add(10);
}
```

### [4\. Overusing Wildcards](#4-overusing-wildcards)[](#4-overusing-wildcards)

While wildcards improve flexibility, using them unnecessarily can make the code harder to understand.

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. What is Generics in Java?](#1-what-is-generics-in-java)[](#1-what-is-generics-in-java)

Generics allow you to define a class or method with a type parameter. For example:

```
class Box<T> {
    private T value;

    public void setValue(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }
}
```

Here, `T` is a type parameter that can be replaced with any concrete type at runtime.

### [2\. Why Use Generics in Java?](#2-why-use-generics-in-java)[](#2-why-use-generics-in-java)

-   **Type Safety**: Prevents ClassCastException at runtime.
    
-   **Code Reusability**: Enables writing a single class or method that works with different types.
    
-   **Improved Readability**: Avoids excessive casting and makes code cleaner.
    

For a deeper understanding of Java fundamentals, check out the [Java EE tutorial](/community/tutorials/java-tutorial-java-ee-tutorials).

### [3\. Can you give an example of a generic method?](#3-can-you-give-an-example-of-a-generic-method)[](#3-can-you-give-an-example-of-a-generic-method)

A generic method can take a type parameter:

```
public static <T> void printArray(T[] elements) {
    for (T element : elements) {
        System.out.println(element);
    }
}
```

This method prints elements of an array of any type.

### [4\. What Does `List<?>` in Java Mean?](#4-what-does-list-in-java-mean)[](#4-what-does-list-in-java-mean)

The `?` is a **[wildcard](https://www.tpointtech.com/wildcards-in-java)** in Java generics. It represents an unknown type. For example:

```
List<?> list = new ArrayList<String>();
```

This means the list can hold any type, but its exact type is unknown at compile time. You can learn more about comparators in java in this tutorial on [Comparable and Comparator in Java Example](/community/tutorials/comparable-and-comparator-in-java-example).

### [5\. Understanding Wildcards in Java Generics](#5-understanding-wildcards-in-java-generics)[](#5-understanding-wildcards-in-java-generics)

There are two main types of wildcards:

1.  `? extends T`: Represents an unknown type that is a subclass of `T`.
    
    ```
       public void processElements(List<? extends Number> numbers) {
     for (Number num : numbers) {
         System.out.println(num);
       }
    }
    ```
    
2.  `? super T`: Represents an unknown type that is a superclass of `T`.
    
    ```
    public void addNumbers(List<? super Integer> numbers) {
     numbers.add(42);
    }
    ```
    

Wildcards provide flexibility while maintaining type safety.

### [6\. Can I Create an Array of Generics in Java?](#6-can-i-create-an-array-of-generics-in-java)[](#6-can-i-create-an-array-of-generics-in-java)

No, Java does not allow generic array creation due to type erasure. This would lead to `ClassCastException` error. Instead, you can use a `List<T>`:

```
List<String> list = new ArrayList<>();
```

For more details on Java type annotations, refer to our tutorial on [Java Annotations](/community/tutorials/java-annotations).

### [7\. Why Are Generics Safe in Java?](#7-why-are-generics-safe-in-java)[](#7-why-are-generics-safe-in-java)

Generics enforce type checking at compile time, reducing the likelihood of runtime errors. They eliminate the need for explicit casting and make code more readable and maintainable.

### [8\. What Are the Rules for Generics in Java?](#8-what-are-the-rules-for-generics-in-java)[](#8-what-are-the-rules-for-generics-in-java)

1.  Cannot create instances of a generic type.
    
    ```
    class Box<T> {
     // T obj = new T(); // Illegal
    }
    ```
    
2.  No static members of generic type.
    
    ```
    class Box<T> {
       // static T instance; // Illegal
    }
    ```
    
3.  Cannot use primitives as type parameters (use wrapper classes instead).
    
    ```
    // List<int> list = new ArrayList<>(); // Illegal
    List<Integer> list = new ArrayList<>();
    ```
    

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

Java Generics offers a robust mechanism to ensure type safety and promote code reusability. By grasping the nuances of wildcards, avoiding common pitfalls, and adhering to best practices, you can craft more maintainable, efficient, and error-free Java applications.

For more Java-related topics, you can refer to our tutorials on [Java EE Tutorials](/community/tutorials/java-tutorial-java-ee-tutorials) and [Comparable and Comparator examples](/community/tutorials/comparable-and-comparator-in-java-example).

#### [Source](https://www.digitalocean.com/community/tutorials/java-generics-example-method-class-interface)

<br/>
---
