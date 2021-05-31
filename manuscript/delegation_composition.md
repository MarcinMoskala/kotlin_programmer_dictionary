# Delegation vs Composition

When I was describing Class Delegation in Android Development in Kotlin, I referenced to great books like “Effective Java” that showed advantages of this solution over inheritance. Although “Effective Java” readers might be confused because the book hasn’t used “delegation” word at all. It was describing advantages of “composition”. So how do its arguments refer to Class Delegation feature?
The answer is: Perfectly. All presented examples showed delegation and composition at the same time. Actually, Delegation pattern needs composition! We can say that Delegation pattern is a one way how we can use composition. These terms were born from different conceptions and we should start from this concepts to understand it.
Composition term is used in terms of object modeling. It represents “has-a” relationship between objects. It is usually contrasted to inheritance and aggregation. When Car is an object, we say that it composes Engine. From a practical point of view, we can say that every read-only property in class is an example of composition. (Read-write property is most likely an example of aggregation.)
Delegation is used in terms of responsibility. When we need to do something, we can delegate this responsibility to another object. It is usually contrasted to dispatching or forwarding.

Image from Diplo
Based on delegation idea, Delegation Pattern was born. In Delegation Pattern, class composes delegate as a read-only property and passes calls of some methods to it. Here is an example:
class Rectangle(val height: Double, val width: Double) {
fun area() = height * width
}

class Square(val a: Double) {
val rectangle = Rectangle(a, a)
fun area() = rectangle.area()
}
This is an example of Delegation pattern. Let’s see how we can use inheritance instead:
open class Rectangle(val height: Double, val width: Double) {
fun area() = height * width
}
class Square(val a: Double): Rectangle(a, a)
When we look at inheritance usage, it might look like better implementation. First of all, it is shorter and simpler. Second of all, we have polymorphic behavior that allows us to accept both Square and Rectangle:
fun printSize(shape: Rectangle) {
println("Size is ${shape.area()}")
}

// Usage
printSize(Rectangle(10.0, 20.0)) // Size is 200.0
printSize(Square(10.0)) // Size is 100.0
To have similar behavior in Delegation pattern, we need to introduce some interface that is common for both delegate and class that delegates:
interface Shape() {
fun area(): Double
}
class Rectangle(val height: Double, val width: Double): Shape {
override fun area() = height * width
}

class Square(val a: Double): Shape {
val rectangle = Rectangle(a, a)
override fun area() = rectangle.area()
}
fun printSize(shape: Shape) {
println("Size is ${shape.area()}")
}

// Usage
printSize(Rectangle(10.0, 20.0)) // Size is 200.0
printSize(Square(10.0)) // Size is 100.0
This is how Delegation pattern is most often implemented. Notice that here, and in most cases, object delegates methods specified in some interface into a delegate. This is why the interface can be used to specify which methods we want to delegate. This is the ideology behind Class Delegation, which require an interface to specify which methods should be delegated:
interface Shape() {
fun area(): Double
}
class Rectangle(val height: Double, val width: Double): Shape {
override fun area() = height * width
}

class Square(val a: Double): Shape by Rectangle(a, a)
Now we have short syntax and polymorphic behavior. But why should we prefer this implementation over one that uses inheritance? I gave the long answer at the beginning of this lecture and in Android Development in Kotlin book. A short answer is: Because Square is not really Rectangle, from object oriented design point of view. If we would write tests for Rectangle, they wouldn’t pass for Square! Square is not extended Rectangle. This is its special case. Inheritance usage breaks principles of the object oriented design. Especially Liskov substitution principle. We can still use Rectangle in Square, but we should use composition instead of inheritance. In this case Delegation pattern, which is the concrete implementation of composition, matches our needs perfectly.