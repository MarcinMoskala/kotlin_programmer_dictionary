# Parameter vs Argument, Type parameter vs Type argument

Parameter and argument are often confused, but they are totally different concepts. Let’s discuss what they are and what are the differences. We will also understand what is type parameter and type argument.

## Parameter vs Argument
Parameter is variable defined in function definition, while argument is actual value passed to the function. To understand the difference, let’s first see an example function and its usage:

```kotlin
fun randomString(length: Int): String {
   // ....
}
randomString(10)
```

In this example length is a parameter, and 10 (used in function call) is an argument. Here are common definitions:

> Parameter is variable defined in function declaration. Argument is the actual value of this variable that get passed to the function.

Conversation example: 
 - `randomString` function is throwing an error. What argument are you passing to length parameter?
 - It is -1
 - And this is the reason. randomString is not prepared for negative values of length parameter!

## Type parameter vs Type argument

Distinction between parameter and argument is universal, and it is applicable for all types of functions: methods, constructors etc. It is also applicable for wording used in generic. Let’s discus example generic class:

```kotlin
class Box<T>
val a: Box<Int> = Box()
```

Here Box is generic class, which defines T type parameter. In use, we specifies Int as a type argument (see here why type, not class). 

Common definitions are following:

> Type parameter is blueprint or placeholder for a type declared in generic. Type argument is actual type used to parametrize generic.