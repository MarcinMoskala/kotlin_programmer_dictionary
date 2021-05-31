# Field vs Property

This is an example of a Java field:

```java
public String name = "Marcin";
```

Here is an example of a Kotlin property:

```kotlin
var name: String = "Marcin"
```

They both look very similar, but these are two different concepts. Direct Java equivalent of above Kotlin property is following:

```java
private String name = "Marcin";
public String getName() {
    return name;
}
public void setName(String name) {
    this.name = name;
}
```

The default implementation of Kotlin property includes field and accessors (getter for val, and getter and setter for var). Thanks to that, we can always replace accessors default implementation with a custom one. For instance, if we want to accept only non-blank values, then we can define the following setter:

```kotlin
var name: String = "Marcin"
    set(value) {
        if (value.isNotBlank())
            field = value
        }

name = "Marcin"
name = ""
print(name) // Prints: Marcin
```

If we want to be sure that the returned property value is capitalized, we can define a custom getter which capitalizes it:

```kotlin
var name: String = "Marcin"
    get() = field.capitalize()

name = "marcin"
print(name) // Prints: Marcin
```

The key fact regarding properties is that they actually are defined by their accessors. A property does not need to include any field at all. When we define custom accessors that are not using any field, then the field is not generated:

```kotlin
var name: String
    get() = "Marcin"
```

This is why we can use property delegation. See example of property delegate below:

```kotlin
var name: String by NameDelegate()
```

Above code is compiled to the following implementation:

```kotlin
val name$delegate = NameDelegate()
var name: String
    get() = name$delegate.getValue(this, this::name)
    set(value) { 
        name$delegate.setValue(this, this::name, value)
    }
```

Moreover, while a property is defined by its accessors, we can specify it in the interface:

```kotlin
interface Person {
    val name: String
}
```

Such declaration means that there must be a getter defined in classes that implement interface Person.

As you can clearly see, Kotlin properties give developers much bigger possibilities than Java fields. Yet, they look nearly the same and Kotlin properties can be used exactly the same as Java fields. This is a great example, how Kotlin hides complexity under the carpet and gives us possibilities even if some developers remain unaware of them.