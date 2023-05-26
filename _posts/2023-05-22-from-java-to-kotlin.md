---
layout: post
title: "From Java to Kotlin: The Basic"
date: 2023-05-22 20:00:00 +0700
categories: Kotlin
---
Kotlin is a versatile, general-purpose programming language that offers multiple target platforms, one of them being the Java Virtual Machine (JVM).  This writing will discussing Kotlin's unique capabilities and advantages within the context of JVM.

And because the JVM as its target, Kotlin code is compiled into Java bytecode, enabling it to interoperate seamlessly with Java. This interoperability means that developers can utilize all existing Java libraries and frameworks while enjoying Kotlin's more modern and expressive syntax, improved safety features, and streamlined development experience.

```
*.java ---> *.class
*.kt ---> *.class
```

If you're transitioning from Java to Kotlin, one of the most beneficial features at your disposal is the automatic conversion of Java code to Kotlin code. This conversion isn't just limited to simple or basic code—it can handle complex Java classes too.

To illustrate this, consider the following Java class, `Person`:

```java
public class Person {
  private final Sring name;
  private final int age;

  public Person(String name, int age) {
    this.name = name;
    this.age = age;
  }

  public String getName() {
    return name;
  }

  public int getAge() {
    return age;
  }
}
```

We can convert it to Kotlin:

```kotlin
class Person(val name: String, val age: Int)
```

Yes, as short as that. Kotlin automatically create the getter.  The `val` keyword in the class definition declares read-only properties. Under the hood it create the implementation. Even better you can add `data` before class, it then will create `equals()`, `hashCode()`, and `toString()` methods.

```kotlin
data class Person(val name: String, val age: Int)
```

In Java, creating an instance of the `Person` class and accessing its properties would typically look like this:

```java
Person person = new Person("Alice", 28);
System.out.println(person.getName());
```

In the Java code above, a new `Person` object is created using the `new` keyword and its properties can be accessed via the provided getter methods.

In contrast, here's how we instantiate the class and access its properties in Kotlin:

```kotlin
val person = Person("Alice", 28)
println(person.name)
```

In the Kotlin code, the instantiation of the `Person` object is noticeably simpler. There is no `new` keyword required for creating an object in Kotlin. Instead, we use the class name directly, just like calling a function. Moreover, Kotlin provides direct access to the properties of the `Person` object, eliminating the need to use explicit getter methods.

The absence of the `new` keyword and direct property access are just a few examples of how Kotlin enhances readability and reduces verbosity compared to Java.

## Idiomatic Kotlin

While it's true that porting from Java to Kotlin is straightforward, the true power of Kotlin lies in its unique style and features, which lead to simpler, more readable code. Let's take a Java method as an example:

```java
// ...
public void updateWeather(int degress) {
  String desription;
  Color color;
  
  if (degress < 10) {
    description = "cold";
    color = BLUE;
  } else if (degrees < 25) {
    description = "mild";
    color = ORANGE;
  } else {
    description = "hot";
    color = RED;
  }
  // ...
}
// ...
```

This method can be directly translated to Kotlin as follows:

```kotlin
fun updateWeather(degress: Int) {
  val description: String
  val color: Color

  if (degress < 10) {
    description = "cold"
    color = BLUE
  } else if (degrees < 25) {
    description = "mild"
    color = ORANGE
  } else {
    description = "hot"
    color = RED
  }
}
```

However, Kotlin provides a way to further streamline this method. With the use of destructuring declarations and if-else expressions, we can eliminate the need to repeat `description` and `color`:

```kotlin
fun updateWeather(degress: Int) {
  val (description: String, color: Color) =
    if (degress < 10) { Pair("cold", BLUE) }
    else if (degrees < 25) { Pair("mild", ORANGE) }
    else { Pair("hot", RED) }
}
```

This is only scratching the surface of Kotlin's capabilities. For instance, variable types in Kotlin are optional because it uses type inference, allowing the compiler to understand the type of a variable from its expression.

For more complex conditional flows, Kotlin provides the `when` expression, which is analogous to Java's `switch` statement. Here's how the previous function can be refactored using `when`:

```kotlin
fun updateWeather(degress: Int) {
  val (description, color) = when {
    degrees < 10 -> Pair("cold", BLUE)
    degress < 26 -> Pair("mild", ORANGE)
    else -> Pair("hot", RED)
  }
}
```

Lastly, for those who find the use of `Pair` less intuitive, Kotlin offers the `to` keyword to pair values together, resulting in code that reads almost like plain English:

```kotlin
fun updateWeather(degress: Int) {
  val (description, color) = when {
    degrees < 10 -> "cold" to BLUE
    degress < 26 -> "mild" to ORANGE
    else -> "hot" to RED
  }
}
```

In conclusion, while it's simple to convert Java code to Kotlin, leveraging the unique features of Kotlin can result in significantly simpler and more readable code."

## Dig Dive to Kotlin Basics

Previously, we discussed converting Java methods to Kotlin functions. However, it's worth noting that while Kotlin supports top-level functions, Java does not have a direct equivalent—instead, methods in Java must belong to a class. 

Let's take a look at a simple 'hello world' program in Kotlin:

```kotlin
fun main() {
  val name = "World"
  println("Hello, $name!")
}
```

In this Kotlin program, we declare a top-level function, `main()`, that doesn't belong to any class. It prints 'hello world' to the console using a `name` variable. Notice the ease of string interpolation in Kotlin, signified by the `$` symbol.

In contrast, Java requires the `main()` method to be part of a class. Also, string interpolation in Java involves concatenation using the `+` operator.

Now, what if we want to interpolate a more complex expression or a `null` value? Here's how it would look in Java:

```java
public static void main(String[] args) {
  System.out.println("Hello, " + null + "!")
}
```

In Kotlin, for expressions and null values, we wrap the expression or value within `${` and `}` for interpolation:

```kotlin
fun main(args: Array<String>) {
  prntln("Hello, ${null}")
}
```

### Kotlin Variables

In Kotlin, there are two ways to declare variables, using `val` and `var`. A `val` (value) is a read-only (immutable) declaration, while a `var` (variable) can be changed (it's mutable).

Consider this example:

```kotlin
val question = "Life is short"
println("$question?")
```

In this case, `question` is a `val`, meaning it cannot be reassigned after its initial assignment. If you try to change `question`:

```kotlin
question = "sure?"
```

You'll encounter a compiler error stating, "`val cannot be reassigned`". This is similar to a final variable in Java, which also can't be reassigned once initialized.

On the other hand, if you need a variable that can change its value, you should use `var`. Here's an example:

```kotlin
var question = "Are you"
question = "So?"
```

In this case, no error will occur. The `question` variable is mutable and can be reassigned as needed. This fundamental distinction between `val` and `var` is one of the ways Kotlin helps to enforce immutability where possible, promoting a safer and more predictable programming environment.

#### Type Inference

Did you notice something about our previous examples? We assigned a `String` value to a variable without explicitly specifying its type. This is because Kotlin is smart enough to infer the type of the variable from the assigned value. This feature is known as "type inference". While you can explicitly declare the type of a variable like this:

```kotlin
var question: String = "Are you"
```

It is generally unnecessary unless you're dealing with more complex situations where type inference might be ambiguous or unclear.

If you attempt to reassign a variable with a different type, you'll encounter an error. For example:


```kotlin
var question = "Are you"
question = 1
```

This is because `question` was initially inferred to be of `String` type, and `String` can't be implicitly converted to `Int` in Kotlin. However, reassigning it to another `String` would work perfectly fine.

Now, what about `val`?

While it's true that `val` represents a read-only reference that can't be reassigned, it does not necessarily mean that the object it refers to is immutable.

Take a look at this example:

```kotlin
val langs = mutableListOf("Kotlin")
language.add("Clojure")
```

Now, `languages` contains `["Kotlin", "Clojure"]`. We were able to add an element to `languages` even though it was declared with `val` because `mutableListOf()` returns a mutable list, meaning its contents can be changed.

On the other hand, if you declare a list using `listOf()`, you will create an immutable list:

```kotlin
val lang = listOf("Java")
```

In this case, trying to add an element to the list will result in an error:

```kotlin
lang.add("Clojure") // it wont' compile
```

Whenever possible, it's a good practice to declare variables with `val`. It makes your code safer by preventing unnecessary mutation and reducing the potential for bugs.

### Function

Kotlin offers several ways to declare functions. Let's start with a simple example:

```kotlin
fun max(a: Int, b: Int): Int {
  return if (a > b) a else b
}
```

This function can be simplified to:

```kotlin
fun max(a: Int, b: Int): Int = if (a > b) a else b
```

Kotlin's type inference allows us to remove the explicit return type `: Int`, as it can infer the return type from the expression:

```kotlin
fun max(a: Int, b: Int) = if (a > b) a else b
```

In Kotlin, functions always return a value. If you write a function that does not return any specific value (like a `void` function in Java), Kotlin actually returns a special object called `Unit`.

```kotlin
fun displayMax(a: Int, b: Int) {
  println(max(a, b))
}
```

Or, you can explicitly specify `Unit` as the return type:

```kotlin
fun displayMax(a: Int, b: Int): Unit {
  println(max(a, b))
}
```

Interestingly, functions in Kotlin can be declared in any scope, which is different from Java.

For example, a top-level function:

```kotlin
fun topLevel() = "top level"
```

A member function in a class:

```kotlin
class MyClass {
  fun memberFn() = "member function"
}
```

A function within a function:

```kotlin
fun parentFunction() {
  fun localFunction() = "local function"
}
```

And you can call a Kotlin funtion from java class. In Java you call it as a static function of the class, which name corresponds to the file name.

For exampe we write kotlin function in a file name `MyFile.kt`:

```kotlin
package kotlinFile

fun kotlinFunction() = 100
```

You would use the following Java code:

```java
package javaFile;

import kotlinFile.MyFileKt;

public class MyJavaClass {
  public static void main(String[] args) {
    MyFileKt.kotlinFunction();
  }
}
```

If you don't like to call with the filename, you can use `@JvmName` annotation in Kotlin.

```kotlin
@file:JvmName("Util")

package kotlinfile

fun kolinFunction() = 100
```

Then in Java, you can call it like so:

```java
package javaFile;

import kotlinFile.MyFileKt;s

public class MyJavaClass {
  public static void main(String[] args) {
    Util.kotlinFunction(); // Notice, we are using "Util" not "MyFileKt"
  }
}
```

In conclusion, Kotlin offers a wide array of options for declaring and using functions, making it a powerful and flexible.

#### Function Invocation

In Kotlin, we can call functions with named arguments, which can greatly improve readability by making it clear what each argument is meant to do. Here's an example:

```kotlin
listOf('a', 'b', 'c').joinToString(separator = "", prefix = "(", postfix = ")")
// (a,b,c)
```

In this case, `joinToString` is called with **named arguments**.

Named arguments are often used in combination with default argument values. For instance:

```kotlin
listOf('a', 'b', 'c').joinToString(postfix = ".")
// 1, 2, 3.
```

Here, the `separator` defaults to `,` and `prefix` defaults to an empty space.

But how do we define a function with default values for its arguments? The syntax is quite straightforward, with the default values specified in the function declaration:

```kotlin
fun displaySeparator(character: Char = '*', size: Int = 10) {
  repeat(size) {
    print(character)
  }
}
```

We can call this function without any arguments:

```kotlin
displaySeparator()
```

Or, we can override the default values by specifying the argument names:

```kotlin
displaySeparator(size = 100)
```

If we want to change the first argument, we can do so without specifying the argument name:

```kotlin
displaySeparator('#')
```

Interestingly, when argument names are provided, we can ignore the original position of the arguments:

```kotlin
displaySeparator(size = 10, character = '$')
```

Without the argument names, incorrect position would lead to a compile-time error.

In Java, to achieve similar behavior, we would need to use method **overloading**. This involves defining multiple methods with the same name but different parameters:

```java
public vlod displaySeparator(char character, int size) { ... }

public void displaySeparator(char character) { ... }

public void displaySeparator() { ... }
```

In conclusion, named and default arguments in Kotlin provide a more readable and concise way to work with functions, reducing the need for function overloading that we typically see in languages like Java.