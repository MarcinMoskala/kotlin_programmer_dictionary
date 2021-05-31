# Object expression vs Object declaration

In previous part, we’ve already described what is an object. The problem is that Kotlin introduced an object as a keyword. This causes some confusion in the programming community. Because of that, I hear often that programmers are erroneously using the object term to describe two other constructs:
* Object declaration
* Object expression

Let’s describe them separately.

## Object declaration

Here is an example of an object declaration:

```kotlin
object HttpService {
    val api = retrofit.create(Api::class.java)
    fun post(url: String) = api.post(url)
}
```

Object declaration is an implementation of the singleton pattern. After the object keyword, we actually define members of the class. This class can have only a single instance and this is why we reference to it by using the name of this object declaration:

```kotlin
HttpService.post("wwww.myurl.com/event")
val service: HttpService = HttpService
service.post("wwww.myurl.com/event")
```

Still, even though they have the same name, object declaration and object created by this object declaration are not the same thing. They shouldn’t be confused. More information about object declaration can be found on Kotlin reference.

## Companion object

Companion object is a brother of object declaration. It works the same, but it takes name from class:

```kotlin
class Connection private constructor() {

    companion object {
        fun create() = Connection()
    }
}

val connection = Connection.create()
```

It is often used the same way as static fields and properties were used in Java, but while it is an instance of the class, it gives more possibilities. I wish to describe them in another article.
Object expression
Object expression is a structure that creates a single instance of an object:

```kotlin
val coords = object {
    var x = 10
    var y = 10
}
```

Note that it also generates type, while we can use members defined in this object expression:

```kotlin
println(coords.x) // Prints: 10
```

It is often used as a substitution to a Java anonymous class:

```kotlin
window.addMouseListener(object : MouseAdapter() {
override fun mouseClicked(e: MouseEvent) {
// ...
}

    override fun mouseEntered(e: MouseEvent) {
        // ...
    }
})
```

More about object expression can be found on Kotlin reference.
Conclusion
Object declaration and object expression both creates a single object (although object declaration creates it lazily), but they are much more then just objects . They also specify an object implementation and generate a type. This is why they should not be mentioned just as an object.
Here are some examples how can we use this terms in the sentences:
I see that you have an object in HttpProvider object declaration
You can use the object created by the object expression to …