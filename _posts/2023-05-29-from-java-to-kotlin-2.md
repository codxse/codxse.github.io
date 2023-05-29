---
layout: post
title: "From Java to Kotlin: The Logic & Loops"
date: 2023-05-22 20:00:00 +0700
categories: Kotlin
---
In the (first part)[/kotlin/2023/05/22/from-java-to-kotlin] of our Kotlin tutorial, we discussed the basic syntax and primitive types of Kotlin. This section will delve deeper into Kotlin's conditional expressions, comparing them with Java.

In Kotlin, `if` is an expression, not a statement. This means it can return a value, which can then be assigned to a variable. For example:

```kotlin
val max = if (a > b) a else b
```

Notice how Kotlin doesn't have a ternary operator like `(a > b) ? a : b` in Java. The `if` expression fulfills this role.

Besides `if`, Kotlin also offers the `when` expression, which is analogous to `switch` in Java:

```kotlin
enum class Color {
  BLUE, ORANGE, RED
}

fun getDescription(color: Color) =
  when (color) {
    BLUE -> println("cold")
    ORANGE -> println("mild")
    RED -> println("hot")
  }
```

Kotlin's `when` doesn't require a `break` statement like Java's `switch`. The `when` expression in Kotlin matches the first suitable condition it finds, and stops.

```java
switch (color) {
  case BLUE:
    System.out.println("cold");
    break;
  case ORANGE:
    System.out.println("mild");
    break;
  default:
    System.out.println("hot");
}
```

In Kotlin, `when` can also check several values at once if they are separated by a comma:

```kotlin
fun responseToInput(input: String) = when (input) {
  "y", "yes" -> "You are agree"
  "n", "no" -> "You are disagree"
  else -> "You don't decide"
}
```

Moreover, `when` can handle any expression, not just constants, for branch conditions. The argument is checked for equality with each branch condition.

```kotlin
fun mix(c1: Color, c2: Color) =
  when (setOf(c1, c2)) {
    setOf(RED, YELLOW) -> ORANGE
    setOf(YELLOW, BLUE) -> GREEN
    setOf(BLUE, VIOLET) -> INDIGO
    else -> throw Exception("Dirty color")
  }
```

For type checks, Kotlin uses `is`, which is similar to Java's `instanceof`:

```kotlin
when (pet) {
  is Cat -> pet.meow()
  is Dog -> pet.woof()
}
```

If you need to capture the subject of `when` in a variable, you can do it like this:

```kotlin
fun getSound() = 
  when (val pet = getThePet()) {
    is Cat -> pet.meow()
    is Dog -> pet.woof()
    else -> "---"
  }
```

Finally, `when` can also be used without an argument to check different conditions:

```kotlin
fun updateWether(degrees: Int) {
  val (description, color) = when {
    degrees < 5 -> "cold" to BLUE
    degrees < 23 -> "mild" to ORANGE
    else -> "hot" to RED
  }
}
```

In conclusion, Kotlin provides a more intuitive and flexible way to handle conditional expressions compared to Java. With the `if` and `when` expressions, you can write more expressive and concise code.

## Loops

Java features loop structures like `while`, `do-while`, and `for`. In Kotlin, the `while` loop works similarly to Java's `while`, so we won't discuss that here. However, the `for` loop in Kotlin is a bit different and offers some interesting capabilities.

In Kotlin, a `for` loop can iterate through a list:

```kotlin
val list = listOf("a", "b", "c")
for (s in list) {
  println(s)
}
```

It can also iterate through a map:

```kotlin
val map = mapOf(1 to "one", 2 to "two", 3 to "three")

for ((key, value) in map) {
  println("$key -> $value")
}
```

There is also an option to iterate through a list with an index:

```kotlin
val list = listOf("a", "b", "c")
for ((index, element) in list.withIndex()) {
  println("$index: $element")
} 
```

Kotlin `for` loops can also work with a range:

```kotlin
for (i in 1..9) {
  println(i)
} 
```

And can even implement a `step`:

```kotlin
for (i in 9 downTo 1 step 2) {
  print(i)
}
```

As you can see, Kotlin's `for` loop is quite flexible and readable. It doesn't require explicit iterator state management as in Java's `for` loop, leading to more elegant and concise code.

## Ranges

Kotlin provides the power of ranges which we've previously glimpsed in the `for` loop. The code `for (i in 1..9)`, for example, uses a range with the `in` keyword. Java doesn't have an equivalent for this `in` syntax.

In Kotlin, `in` is primarily used in two cases:

**1. For iteration:**

```kotlin
for (i in 'a'..'z') {
  ...
}
```

**2. For checking if a value belongs within a certain range:**

```kotlin
c in 'a'..'z'
```

```kotlin
fun isLetter(c: Char) = c in 'a'..'z' || c in 'A'..'Z'
```

We can also use `in` in `when` expressions to determine which branch to execute:

```kotlin
for recognize(c: Char) = when (c) {
  in '0'..'9' -> "a digit"
  if 'a'..'z', in 'A'..'Z' -> "a letter"
  else -> "Only God know"
}
```

### Other Types of Ranges

Ranges can be created with any comparable elements:

```kotlin
1..9                 // IntRange
1 until 10           // IntRange
'a'..'z'             // CharRange
"ab".."az"           // ClosedRange<String>
startDate..endDate   // ClosedRange<MyDate>
```

These ranges provide flexibility and readability when dealing with sequences or intervals of values. They're a unique and powerful feature of Kotlin that sets it apart from Java.s
