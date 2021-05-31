# Function literal with receiver vs Function type with receiver

We’ve already discussed difference between function literal and function type. Although, Kotlin introduced extension functions and it needed also special type and literals to work with them. They are called function literal with receiver and function type with receiver.

Let’s start from the beginning. Here is a simple example of extension function:

```kotlin
fun Int.square() = this * this
// Usage
println(2.square()) // Prints: 4
```

We’ve told that argument with Int is passed to function’s extension receiver, and this or implicit calls reference to it. If we need to associate this function with the property then we can use function reference:

```kotlin
val squareFun: Int.()->Int = Int::square
```

We’ve used Int.()->Int that is function type with receiver. Instead of using function reference, we can define function using one of the function literals with receiver. This is how we can define it using lambda expression with receiver:

```kotlin
val squareFun: Int.()->Int = { this * this }
```

This is how we can define it using anonymous function with receiver:

```kotlin
val squareFun: Int.()->Int = fun Int.() = this * this
```

Note that anonymous function with receiver have different notation than standard anonymous function, so when it is used then property or variable type can be inferred:

```kotlin
val squareFun = fun Int.() = this * this
```

Lambda expression with receiver doesn’t have such capability so it needs explicit type declaration:

(obrazek https://blog.kotlin-academy.com/programmer-dictionary-function-literal-with-receiver-vs-function-type-with-receiver-cc21dba0f4ff)
Error because lambda expression does not have receiver by default and this is not allowed in this context (there is no implicit receiver)

Lambda expression with receiver is really important in Kotlin because it is strongly used in Kotlin DSL. Here is an example of DSL used to define listener with different handlers:

(obrazek https://blog.kotlin-academy.com/programmer-dictionary-function-literal-with-receiver-vs-function-type-with-receiver-cc21dba0f4ff)
This listener creation was introduced and described in Cleaning up Android project using Kotlin

Thanks to that fact, we can use function type with receiver and lambda expression with receiver, we can strictly specify what handlers can be defined (here onChildAdded or onCancelled) and how we define them. Implementation of childEventListener can be found on this gist.