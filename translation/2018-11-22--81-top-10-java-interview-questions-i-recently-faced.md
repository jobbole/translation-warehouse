---
editor: AtlantisLee
published: false
title: '#81-Top 10 Java Interview Questions I Recently Faced'
---
Need help getting ready for your next Java interview? Check out this post on the top 10 Java interview questions to make sure you're prepared!

Recently, I participate in a few Java-based interviews to keep me fresh. Suddenly, I had an idea that I would like to share my experiences with you all. I hope I can help by sharing the top 10 Java interview questions that I faced in the recent months.

## Top 10 Java Interview Questions I Recently Faced

In this presentation, I tried to collect you the most interesting and common questions. Furthermore, I will provide you with the correct answers. 

So, let’s take a look at these questions.

### 1. Evaluate Yourself on a Scale of 10 — How Good Are you in Java?
This is a very tricky one if you are not exactly sure about yourself or your level of proficiency in Java. If that sounds familiar, you should shoot a bit lower. After this, you will probably get questions according to the level you've admitted to. Hence, if you, for example, said 10 and cannot answer a fairly difficult question, it will be a drawback. 

### 2. Explain the Differences Between Java 7 and 8.
To be honest, there are a lot of differences. Here, if you can list the most significant ones, it should be enough. You should explain the new features in Java 8. For the full list, visit the original website here: [Java 8 JDK](https://www.oracle.com/technetwork/java/javase/8-whats-new-2157071.html).

The most important ones that you should know are:

- **Lambda expressions**, a new language feature, has been introduced in this release. Lambda expressions enable you to treat functionality as a method argument or code as data. Lambda expressions let you express instances of single-method interfaces (referred to as functional interfaces) more compactly.

- **Method references** provide easy-to-read lambda expressions for methods that already have a name.

- **Default methods** enable new functionality to be added to the interfaces of libraries and ensure binary compatibility with code written for older versions of those interfaces.
 
- **Repeating annotations** provide the ability to apply the same annotation type more than once to the same declaration or type use.

- **Type annotations** provide the ability to apply an annotation anywhere a type is used and not just on a declaration. Used with a pluggable type system, this feature enables improved type checking of your code.


### 3. Which Type of Collections Do you Know About?
Here you should know about the most important ones:

- `ArrayList` 
- `LinkedList` 
- `HashMap` 
- `HashSet`

After this, you will probably get some questions about when you should use this specific one, what are the benefits over the other one, how it stores data, and what data structure is working behind the scenes.

Here, the best way is to learn about these collection types as much as possible, because the variety of questions is almost inexhaustible.

### 4. What Methods Does the Object Class Have?
This a very common question asked to determine how familiar you are with the basics. These are the methods that every object has:

The `Object` class, in the `java.lang` package, sits at the top of the class hierarchy tree. Every class is a descendant, direct or indirect, of the `Object` class. Every class you use or write inherits the instance methods of `Object`. You need not use any of these methods, but, if you choose to do so, you may need to override them with code that is specific to your class. The methods inherited from `Object` that are discussed in this section are:

- `protected Object clone() throws CloneNotSupportedException`  
Creates and returns a copy of this object.
- `public boolean equals(Object obj)`   
Indicates whether some other object is “equal to” this one.
- `protected void finalize() throws Throwable`  
Called by the garbage collector on an object when garbagecollection determines that there are no more references to the object.
- `public final Class getClass()`  
Returns the runtime class of an object.
- `public int hashCode()`  
Returns a hash code value for the object.
- `public String toString()`  
Returns a string representation of the object.

The `notify`, `notifyAll`, and `wait` methods of `Object` all play a part in synchronizing the activities of independently running threads in a program, which is discussed in a later lesson and won’t be covered here. There are five of these methods:

- `public final void notify()`
- `public final void notifyAll()`
- `public final void wait()`
- `public final void wait(long timeout)`
- `public final void wait(long timeout, int nanos)`


### 5. Why Is the String Object Immutable in Java?
1. [String pool](https://www.journaldev.com/797/what-is-java-string-pool) is possible only because String is immutable in Java. This way, Java Runtime saves a lot of Java heap space, because different String variables can refer to the same String variable in the pool. If String is not immutable, then String interning would not have been possible, because if any variable would have changed the value, it would have been reflected in other variables.

2. If String is not immutable, then it would cause a severe security threat to the application. For example, database usernames and passwords are passed as String to get the database connection, in-[socket programming](https://www.journaldev.com/741/java-socket-programming-server-client) host, and port details passed as String. Since String is immutable, it's value can’t be changed. Otherwise, any hacker could change the referenced value to cause security issues in the application.

3. Since String is immutable, it is safe for [multithreading](https://www.journaldev.com/1079/multithreading-in-java), and a single String instance can be shared across different threads. This avoids the usage of synchronization for thread safety; Strings are implicitly thread safe.

4. Strings are used in the [Java classloader](https://www.journaldev.com/349/java-classloader), and immutability provides security that the correct class is getting loaded by the `Classloader`. For example, think of an instance where you are trying to load the `java.sql.Connection`  class but the referenced value is changed to `myhacked.Connection` class and can do unwanted things to your database.

5. Since String is immutable, its hashcode is cached at the time of creation, and it doesn’t need to be calculated again. This makes it a great candidate for a key in a map, and it’s processing is fast than other `HashMap` key objects. This is why String is the most-used object of `HashMap` keys.

[Why is String immutable in Java?](https://www.journaldev.com/802/string-immutable-final-java) Click here to learn more.

### 6. What Is the Difference Between Final, Finally, and Finalize?
This question is my favorite one.

- The final keyword is used in several contexts to define an entity that can only be assigned once.
- **The Java** `finally` **block** is a block that is used to execute important code, such as closing connection, stream, etc. The Java `finally` block is always executed, whether the exception is handled or not. Java `finally` block follows the `try` or `catch` block.
- This is a **method** that the `GarbageCollector` always calls just **before** the deletion/destroying the object, which is eligible for Garbage Collection to perform **clean-up activity**.

### 7. What Is the Diamond Problem?
The diamond problem reflects why we are not allowed to do multiple inheritances in Java. If there are two class that have a shared superclass with a specific method, it is overridden in both subclasses. Then, if you decide to inherit from that two `subClasses` , then if you would like to call that method, the language can’t decide which one you would like to call.

![blockchain](https://i2.wp.com/www.zoltanraffai.com/blog/wp-content/uploads/2018/08/diamond-problem-multiple-inheritance.png?w=570&ssl=1)

We refer to this problem as the _diamond problem_. It gets its name from the above image, which describes the caveat.

### 8. How Can You Make a Class Immutable?
I think this is a quite difficult question. You need to do several modifications on your class to achive immutability:

1. Declare the class as final so it can’t be extended.
1. Make all fields private so that direct access is not allowed.
1. Don’t provide setter methods for variables
1. Make all**mutable fields final** so that it’s value can be assigned only once.
1. Initialize all the fields via a constructor performing a deep copy.
1. Perform cloning of objects in the getter methods to return a copy rather than returning the actual object reference.


### 9. What Does Singleton Mean?
A singleton is a class that allows only a single instance of itself to be created and gives access to that created instance. It contains static variables that can accommodate unique and private instances of itself. It is used in scenarios when a user wants to restrict instantiation of a class to only one object. This is helpful usually when a single object is required to coordinate actions across a system.

### 10. What Is a Dependency Injection?
This is the **number one question** you have to know if you work in Java EE or Spring. You can check out my article explaining this further here: [What is a Dependency Injection?](https://www.zoltanraffai.com/blog/different-dependency-injection-techniques/)

### Summary
In this article, we talked about the top 10 Java interview questions, which are, I think, the most important nowadays based on my experiences. If you know about these, I’m sure that you will have a big advantage during your recruitment process.

Hope I could help you! If you have similar experiences in this topic, or you have some success stories, don’t hesitate to share them in the comments below.

Cheers!
