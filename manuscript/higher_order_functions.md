# Higher Order Function

In previous part we’ve discussed most important terms of Kotlin functional programming. In this part we are going to discuss very popular functional programming term: Higher order function. Here is a definition:

A higher order function is a function that takes a function as an argument, or returns a function.

Yes, it is possible in Kotlin ;)

Here is an example of higher order function that takes function as an argument:

```kotlin
fun sendData(data: Data, callback: ()->Unit) {
    thread {
        api.sendData(data)
        callback()
    }
}
```

Here is an example of higher order function that returns a function:

```kotlin
fun makeLogger(loggerFor: Any) = fun(e: Error) {
    Log.e(loggerFor::class.simpleName, e.message ?: "Error", e)
}
// Usage in MainActivity
val logger = makeLogger(this)
val error = Error("Simple error")
logger(error) // Logs: MainActivity: Simple error ...
```

Analogically we use term first order functions to reference function that doesn’t take a function as an argument or return a function as output.

Higher order functions are very important in Kotlin while their usage is an important improvement over older languages. Nearly all functions used to process collections are higher-order:

```kotlin
students.sortedBy { it.grade }
.take(10)
.map { it.name }
```

Kotlin stdlib includes also different extension higher order functions like let, run, apply, also, takeIf and takeUnless. Similarly, like collection processing functions, they are often used all around the projects.

Next thing to notice is that many important Kotlin features are included to support higher order functions:

1. Inline functions (introduced to improve efficiency of higher order functions)
2. Implicit name of the single parameter (rarely useful for defining functions as a value because type inference is not possible, but extremely useful in lambdas provided as arguments)
3. Last lambda in the argument convention (another way to support passing functions as an argument)

Fact that such an important features exist to support higher order functions clearly shows how important they are. Let’s be honest, we are using them all around the Kotlin code and without them it would be impossible to implement DSL or stream processing. They are great and we love them ;)

Subject of higher order functions and their importance in Kotlin I deeply described in chapter 5 of Android Development with Kotlin.
