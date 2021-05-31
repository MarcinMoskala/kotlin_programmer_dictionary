# Event listener vs event handler

Event listener and event handler are two terms that cause confusion. Especially in Kotlin, where the boundary between
them was blurred. Here, I am trying to make it simple.

The general definition of listeners and handlers is much wider than its use in Android. Here are popular definitions:

> A listener watches for an event to be fired.

> The handler is responsible for dealing with the event.

It might be confusing because often the same object listens for the events and handles them. Although it is usually
assumed that when we set the anonymous class as a listener, its methods are actual handlers:

```kotlin
cancelImage.setOnClickListener(object : View.OnClickListener { //1
    override fun onClick(v: View?) { // 2
        dismiss()
    }
})
```

1. Anonymous class is here used as a listener
2. Method onClick is here event handler

We can use named classes as a listeners:

```kotlin
class OnCancelSnackListener(
        val snackbar: Snackbar
) : View.OnClickListener {
    override fun onClick(v: View?) {
        snackbar.dismiss()
    }
}

// usage
cancelImage.setOnClickListener(OnCancelSnackListener(this))
```

Listeners are often objects, so they are commonly suffixed with Listener. Handlers are normally not suffixed, but
instead, they most often have on as the prefix (onClick, onSwipe etc.). Note, that is is more problematic when we set
listener using lambda expression:

```kotlin
cancelImage.setOnClickListener { dismiss() }
```

What is this lambda expression? Listener or handler? Formally it specifies how the event should be handled. The listener
object is generated under the hood by Kotlin. Does it mean that when we name function that accepts handler then we
should name it using Handler suffix (like setOnClickHandler)? Not at all. Although you can find such approaches in some
JS libraries, like ExJS:

```js
handler: function() {
    ...
}

listeners: {
    'click': function() {
        ...
    }
}
```

Convention in Java, and I believe most other languages, is to use Listener suffix for historical reasons! You can find
out on Kotlin stdlib and JetBrains Kotlin code that Kotlin also adopt that convention. So you can confidently define
following functions:

```kotlin
fun setOnLoadedListener(handler: () -> Unit) {
    // Code
}
fun addOnFlingListener(handler: () -> Unit) {
    // Code
}
```

As well as you can define following functions:

```kotlin
fun setOnLoadedListener(listener: () -> Unit) {
    // Code
}
fun addOnFlingListener(listener: () -> Unit) {
    // Code
}
```

Just remember that it is a good practice to state one convention for the project and respect it.
