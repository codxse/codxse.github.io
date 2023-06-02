---
layout: post
title: "From Java to Kotlin: Exceptions"
date: 2023-06-02 15:00:00 +0700
categories: Kotlin
---
Exception handling in Kotlin is quite similar to that in Java. However, a significant difference is that in Kotlin, there is no distinction between checked and unchecked exceptions.

In Kotlin, `throw` is an expression, which means it can be used in conditional statements. Take a look at the following code:

```kotlin
val percentage =
  if (number in 0..100)
    number
  else
    throw IllegalArgumentException(
      "a percentage value must between 0 amd 100"
    )
```

In this snippet, we're throwing an exception directly as part of an `if` expression. In contrast, Java treats `throw` as a statement, meaning it can't be assigned to a variable or used in this way.


Similarly, `try` is also an expression in Kotlin:
```kotlin
val number = try {
  Integer.parseInt(string)
} catch (e: NumberFormatException) {
  return
}
```

In this case, we are using a `try-catch` block as an expression that can be assigned to a variable. If the `try` block fails and a `NumberFormatException` is thrown, the catch block is executed, and the function will return early.


Unlike Java, Kotlin does not require that exceptions be declared. However, if you are interoperating with Java and want to declare that a function might throw an exception, you can use the `@Throws` annotation:

```kotlin
@Throws(IOException::class)
fun foo() {
  throw IOException()
}
```

In this example, the function `foo` is declared to potentially throw an `IOException`. This information would be useful for Java code calling the Kotlin function.