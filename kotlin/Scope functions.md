### 作用域函数
### `let`, `apply`, `run`, `also`, `with`

## `let`
当您想要在代码中执行空检查并随后使用返回的对象执行进一步的操作时，请使用 let scope 函数。

```kotlin
fun sendNotification(recipientAddress: String): String {  
    println("Yo $recipientAddress!")  
    return "Notification sent!"  
}  
  
fun getNextAddress(): String {  
    return "sebastian@jetbrains.com"  
}
```

```kotlin
val address: String? = getNextAddress()
val confirm = if(address != null) {
    sendNotification(address)
} else { null }
```

```kotlin
val address: String? = getNextAddress()
val confirm = address?.let {
    sendNotification(it)
}
```

```kotlin
address?.let {
    sendNotification(it)
}
```

https://kotlinlang.org/docs/kotlin-tour-intermediate-scope-functions.html#let

通过这种方法，您的代码可以处理可能为空值的`地址`变量，并且您可以稍后在代码中使用`确认`变量。

`apply`
在创建对象时初始化
```kotlin
class Client() {
    var token: String? = null
    fun connect() = println("connected!")
    fun authenticate() = println("authenticated!")
    fun getData(): String = "Mock data"
}

val client = Client()

fun main() {
    client.token = "asdf"
    client.connect()
    // connected!
    client.authenticate()
    // authenticated!
    client.getData()
}
```

使用`apply`作用域函数在创建对象（例如类实例）时初始化它们，而不是在代码后期再初始化。这种方法使你的代码更易于阅读和管理。

```kotlin
val client = Client().apply {
    token = "asdf"
    connect()
    authenticate()
}

fun main() {
    client.getData()
    // connected!
    // authenticated!
}
```
https://kotlinlang.org/docs/kotlin-tour-intermediate-scope-functions.html#apply

于创建时初始化相关变量，调用相关函数
主函数仅需调用最后一步

### `run`
与`apply`类似，你可以使用`run`作用域函数来初始化一个对象，
但最好使用`run`在代码中的特定时刻初始化一个对象并立即计算结果。
```kotlin
class Client() {
    var token: String? = null
    fun connect() = println("connected!")
    fun authenticate() = println("authenticated!")
    fun getData(): String = "Mock data"
}

val client: Client = Client().apply {
    token = "asdf"
}

fun main() {
    val result: String = client.run {
        connect()
        // connected!
        authenticate()
        // authenticated!
        getData()
    }
}
```
https://kotlinlang.org/docs/kotlin-tour-intermediate-scope-functions.html#run

在完成必要的初始化操作后，直接执行剩下的步骤
`.run` 体内的函数会依次执行，并将结果赋值给`result`

