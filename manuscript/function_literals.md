# Function Type vs Function literal vs Lambda expression vs Anonymous function

In Kotlin, functions are first-class citizen. It means that functions can be assigned to the variables, passed as an arguments or returned from another function. While Kotlin is statically typed, to make it possible, functions need to have a type. It exists and it is called function type. Here are a few examples:

* `()->Unit` — the function type that returns nothing useful (Unit) and takes no arguments.
* `(Int)->Int` — the function type that returns Int and takes single argument of type Int.
* `()->()->Unit` — the function type that returns another function that returns nothing useful (Unit). Both functions take no arguments.

Function type is just a syntactic sugar for an interface, but the interface cannot be used explicitly. Nevertheless we can use function types like interfaces, what includes using them as type arguments or implementing them:

```kotlin
class MyFunction: ()->Unit {

    override fun invoke() {
        println("I am called")
    }
}

fun main(args: Array<String>) {
    val function = MyFunction()
    function() // Prints: I am called
}
```

We can also use them to type local variables, properties or arguments:

```kotlin
val greet: ()->Unit
val square: (Int)->Int
val producePrinter: ()->()->Unit
```

None of the above variables contain any value. Let’s specify some. The simplest way to provide function is to use function reference that is referencing to the actual function. This is how it can be used:

```kotlin
fun greetFunction() {
println("Hello")
}
val greet = ::greetFunction
```

Function reference is an example of reflection. It returns reference to the function which also implements an interface that represent function type. This is why it can be used this way.

Another way to provide a function is to use function literal. Generally, literal in programming is a syntactic sugar for representing values of some types the language considers particularly important. Therefore function literal is a special notation used to simplify how a function is defined. There are two types of function literals in Kotlin:
* Lambda expression
* Anonymous function

Lambda expression is a short way to define a function. Let’s use it to fill the above variables:

```kotlin
val greet: ()->Unit = { println("Hello") }
val square: (Int)->Int = { x -> x * x }
val producePrinter: ()->()->Unit = { { println("I am printing") } }
// Usage
greet() // Prints: Hello
println(square(2)) // Prints: 4
producePrinter()() // Prints: I am printing
```

Note that argument type in square was inferred from property type. We could instead type it explicitly and then property type could be inferred from lambda expression. Similarly lack of arguments in greet and in producePrinter is enough to infer property type:

```kotlin
val greet = { println("Hello") }
val square = { x: Int -> x * x }
val producePrinter = { { println("I am printing") } }
```

Anonymus function is an alternative way to define a function. Let’s use it to fill variables:

```kotlin
val greet: ()->Unit = fun() { println("Hello") }
val square: (Int)->Int = fun(x) = x * x
val producePrinter: ()->()->Unit = fun() = fun() { println("I am printing") }
```

Properties types can be similarly inferred:

```kotlin
val greet = fun() { println("Hello") }
val square = fun(x: Int) = x * x
val producePrinter = fun() = fun() { println("I am printing") }
```

As we can see, lambda expression and anonymous functions are pretty similar. Why are they distinguished? Generally the big difference is that anonymous functions are more explicit. It is more clear when we are using them, and return value needs to be specified explicitly. Lambda expression returns value of the last statement in its body or Unit. Unlabeled return is not working there:

```kotlin
val getMessage = { response: Response ->
    if(response.code !in 200..299) {
        return "Error" // Error! Not allowed
    }
    response.message
}
```

We have to use labeled return to finish lambda expression earlier than in the last statement:

```kotlin
val getMessage = lambda@ { response: Response ->
    if(response.code !in 200..299) {
        return@lambda "Error"
    }
    response.message
}
```

Anonymous functions are acting like normal functions and both return type and return statement needs to be explicit:

```kotlin
val getMessage = fun(response: Response): String {
    if(response.code !in 200..299) {
        return "Error" // Returns from getMessage
    }
    return response.message
}
```

Fact is that this notations can be used interchangeably, but it is better to use anonymous functions when we need to use return more then once. Lambda expression should be preferred for small functions with single expression. Although there are some cases when it is better to use anonymous function instead. We all know common puzzler:

```kotlin
fun greet() = { println("Hello") }
greet() // What does it print?
```

Answer is “Nothing, because greet returns function instead of printing anything”. Now notice how obvious the answer would be if user would use anonymous function instead of lambda expression:

```kotlin
fun greet() = fun() { println("Hello") }
```

This is generally the case where anonymous function is better because it is more explicit. Similarly, we should use anonymous function when we want to highlight that last statement is a return type. These are cases when we prefer explicit way that anonymous functions gives us.