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

### `also`

使用 also 范围函数对对象完成附加操作，然后返回该对象以继续在代码中使用它，例如编写日志。

```kotlin
fun main() {
    val medals: List<String> = listOf("Gold", "Silver", "Bronze")
    val reversedLongUppercaseMedals: List<String> =
        medals
            .map { it.uppercase() }
            .filter { it.length > 4 }
            .reversed()
    println(reversedLongUppercaseMedals)
    // [BRONZE, SILVER]
}
```

```kotlin
fun main() {
    val medals: List<String> = listOf("Gold", "Silver", "Bronze")
    val reversedLongUppercaseMedals: List<String> =
        medals
            .map { it.uppercase() }
            .also { println(it) }
            // [GOLD, SILVER, BRONZE]
            .filter { it.length > 4 }
            .also { println(it) }
            // [SILVER, BRONZE]
            .reversed()
    println(reversedLongUppercaseMedals)
    // [BRONZE, SILVER]
}
```

在执行期间加入also 可以查看其内容
就像如上所属适合编写日志

### `with`

与其他作用域函数不同，with 不是扩展函数，因此语法不同。您将接收者对象作为参数传递给 with。
该示例创建 `mainMonitorPrimaryBufferBackedCanvas` 作为 Canvas 类的实例，然后使用不同的函数参数对该实例调用一系列成员函数。


```kotlin
class Canvas {
    fun rect(x: Int, y: Int, w: Int, h: Int): Unit = println("$x, $y, $w, $h")
    fun circ(x: Int, y: Int, rad: Int): Unit = println("$x, $y, $rad")
    fun text(x: Int, y: Int, str: String): Unit = println("$x, $y, $str")
}

fun main() {
    val mainMonitorPrimaryBufferBackedCanvas = Canvas()

    mainMonitorPrimaryBufferBackedCanvas.text(10, 10, "Foo")
    mainMonitorPrimaryBufferBackedCanvas.rect(20, 30, 100, 50)
    mainMonitorPrimaryBufferBackedCanvas.circ(40, 60, 25)
    mainMonitorPrimaryBufferBackedCanvas.text(15, 45, "Hello")
    mainMonitorPrimaryBufferBackedCanvas.rect(70, 80, 150, 100)
    mainMonitorPrimaryBufferBackedCanvas.circ(90, 110, 40)
    mainMonitorPrimaryBufferBackedCanvas.text(35, 55, "World")
    mainMonitorPrimaryBufferBackedCanvas.rect(120, 140, 200, 75)
    mainMonitorPrimaryBufferBackedCanvas.circ(160, 180, 55)
    mainMonitorPrimaryBufferBackedCanvas.text(50, 70, "Kotlin")
}
```

```kotlin
val mainMonitorSecondaryBufferBackedCanvas = Canvas()
with(mainMonitorSecondaryBufferBackedCanvas) {
    text(10, 10, "Foo")
    rect(20, 30, 100, 50)
    circ(40, 60, 25)
    text(15, 45, "Hello")
    rect(70, 80, 150, 100)
    circ(90, 110, 40)
    text(35, 55, "World")
    rect(120, 140, 200, 75)
    circ(160, 180, 55)
    text(50, 70, "Kotlin")
}
```

区别在于在with体内直接调用拓展函数
而不必每次对实例调用推展函数

