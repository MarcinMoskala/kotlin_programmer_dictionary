# Statement vs Expression

Statement and expression are two important terms which are commonly misunderstood. Let’s start explanation from the expression term.

## Expression

The expression term in Kotlin community is often associated with Kotlin single-expression functions:

```kotlin
fun bigger(a: Int, b: Int) = if(a > b) a else b
```

With this in mind, we should intuitively know what are expressions. Problem is that intuition might be misleading. Let’s start instead from one of the common definitions:

> An expression in a programming language is a combination of one or more explicit values, constants, variables, operators and functions that the programming language interprets and computes to produce another value.

Therefore, `1 + 1` is an expression. The same as `sumOf(1, 2, 3)`. Note, that an expression can contain another expression. For example, the expression `sumOf(1, 2 * 3)` contains the expression `2 * 3` (as well as `1`, `2` and `3`). An expression is every part of code that returns value. In Kotlin every function returns at least `Unit`, therefore every function invocation is an expression. Does it mean that everything is an expression? Definitely not! Here are a few examples:

* Variable declaration is not an expression (val a = 10)
* Variable or property assignment is not an expression in Kotlin (a = 10)
* Local class declaration is not an expression (class A {})

They are not expressions, but they are all statements.

## Statement

Let’s start our explanation from another common definition:

> In computer programming, a statement is the smallest standalone element of an imperative programming language that expresses some action to be carried out.
I guess that it is not clear, so I will show an example. Let’s look at the following code:

```kotlin
val bestUser = users.filter { it.passing }
    .maxBy { it.meanScore }
println("${bestUser.name} ${bestUser.surname}")
```

I see here a lot of expressions, but only two statements. First is processing users collection and storing result in the bestUser variable. Second statement is displaying name and surname of this user. The simplest heuristic is that a statement is a part of code which was finished by a semicolon in Java. In Kotlin it is often a single line of code, but we can also write multiple statements in a single line (if we use a semicolon) or we can use multi-lined statements:

```kotlin
val bestUsers = users.filter { it.passing }
    .sortedBy { -it.meanScore }
    .take(10)
print("The best students are: "); println(bestUsers.joinToString());
```

Here are 3 statements. First is processing users to get the best users, second is printing “The best students are: “, and third is printing the best users jointed to a single string.

Note that a standalone expression is also a statement. Like below updateUser function call:

```kotlin
updateUser(user)
```

Such standalone expression is called an expression statement.

Interesting observation is that in purely functional programming there are no statements. There are only expressions.

## Why do you need it?

Now when you understand what is a statement and an expression, you should see how useful this terms are when you describe some code in a book or in an article. Let’s see an example:

```kotlin
fun showUsers(users: List<User>?) {
users ?: return
val adapters = users.map { UserAdapter(it, ::onUserClicked) }
list.adapter = UserListAdapter(adapters)
}
```

In above code snippet, we can see how showUsers is defined. In the first statement of its body, it is checking if the users parameter is not null. Notice that while function parameters in Kotlin are read-only, such assertion is smart casting users into non-nullable for the rest of the function. Therefore in the second statement, we can just use it without any unpacking. Note that when we are mapping users into adapters on the expression UserAdapter(it, ::onUserClicked) we also provide the argument with reference to function onUserClicked. In the last statement we are specifying an adapter of the list as a newly created instance of UserListAdapter. It includes adapters created for all users.

Notice that the words statement and expression are helping us to specify what do we mean when we describe code.

## What is an expression in Java vs in Kotlin?

Note, that there are some fundamental differences between what is, and what is not an expression in Kotlin and in Java. All Kotlin functions calls are expressions, because they return at least Unit. Calls of Java functions that do not defined any return type are not expressions. Kotlin value assignment (a = 1) is not an expression in Kotlin, while it is in Java because over there it returns assigned value (in Java you can do a = b = 2 or a = 2 * (b = 3)). All usages of control structures (if, switch) in Java are not expressions, while Kotlin allowed if, when and try to return values:

```kotlin
val bigger = if(a > b) a else b

val color = when {
    relax -> GREEN
    studyTime -> YELLOW
    else -> BLUE
}

val obj = try { 
    gson.fromJson(json)
} catch (e: Throwable) {
    null
}
```