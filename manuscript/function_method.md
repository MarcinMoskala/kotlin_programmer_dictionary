# Function vs Method vs Procedure

Function and method are two commonly confused words. While every method is a function, not every function is a method. There is also another word that is erroneously used interchangeably: a procedure. Let’s discuss differences between all of this terms.

## Function vs Procedure

Distinction between function and procedure was important in older programming languages. For example, in Pascal functions and procedures are defined using different keywords. Formal difference between this concepts is following:

> Function returns a value, while procedure doesn’t.

If they are so similar then why they were differentiated at all? The real reason was that good practices from functional and procedural programs design claimed that procedures should be used only to invoke a set of operations, and functions should be used just to calculate return value (they should have no side-effects). This good design practices were strongly abused. Developers were using procedures to modify objects provided by an argument (e.g. sort(list)) and functions to invoke some operations and return some data that informs about result (e.g. int success = saveData(data)). This way distinction between function and procedure was obliterated.

Can we still define a procedure? We can do it in Java, because every method that doesn’t specify return type is a procedure:

```kotlin
void printUser(User user) {
    System.out.println(user);
}
```

In Kotlin there are no procedures, because every function returns at least `Unit` object. Note that direct analogue of `printUser` procedure is a function in Kotlin, because it returns `Unit`:

```kotlin
fun printUser(user: User) {
    println(user)
}
val result = printUser(user)
print(result) // kotlin.Unit
print(result is Unit) // Prints: true
```

Some call such functions "procedures", and I see nothing wrong with that. But you can safely forget about this word in Kotlin and you will be formally correct. This is good, because the less industry terms you use, the more understandable you are for average audience.

## Function vs method

Another big confusion is between function and method. Difference is following:

> Method is a function associated to an object.

Function is a more general term, and all methods are also functions. What are methods then? Definitely all member functions and member property accessors are methods:

```kotlin
class A {
    fun someMethod() {}
}
```

In OOP, classes have members that represent their data and behavior. A member of a Kotlin class is either a method or a property. This is why properties defined in the class are called a member property in contrast to top-level properties that are not associated to any class.

What about Java static methods? This is debatable. On the one hand, Java terminology most often names them methods. On the other hand, lot’s of university authoritarians argues that they are not, because they are associated to a class, not to an object. Definitely all top-level functions are not methods.

A bit more problematic are functions of companion objects and object declarations. Companion object functions are commonly used as an analogues of Java static functions, which are not methods:

```kotlin
class MainActivity: Activity() {

    companion object {
        fun start(context: Context) {
            val intent = Intent(this, MainActivity::class.java)    
            context.startActivity(intent)
        }
    }
}
```

From the other hand, companion object is actually an object declaration. Object declaration is actually Kotlin implementation of singleton pattern and all its functions are associated to this singleton object instance. Therefore they are methods.

What about extension functions? Member extension functions, like all member functions, are methods. Top-level extension functions are compiled into static functions with receiver placed as a first parameter. They are not members. On the other hand, it is common to say that they are associated with the receiver type and based on this reasoning we can call them methods as well. On C# nomenclature they are called extension methods and I think it is totally reasonable to name them here this way.

Note that Kotlin uses fun keyword which is a shortcut from function. This is absolutely correct while function is more general word which includes methods, and there are no procedures in Kotlin. Smart :)

In our wording we might always use function, because this is more general word that includes methods. Although it is a good practice to use method term wherever it is more adequate. The reason is the same as when we are saying "dog" instead of “animal” when we know that we are talking about a dog. It provides more information. By making our speech more concrete, we makes it more understandable.