```kotlin
class Contact(val id: Int, var email: String) { 
val category: String = "Kotlin" 
}
```

为类声明属性

```kotlin
val id: Int, var email: String
```

可具备默认值

```kotlin
var email: String = "example@gmail.com"
```

实例化

```kotlin
val contact = Contact(1, "mary@gmail.com")
```

访问

```kotlin
println(contact.email)
```

字符串模板

```kotlin
println("Their email address is: ${contact.email}")
```

成员函数
其中可套函数娃
```kotlin
class Contact(val id: Int, var email: String) {
    fun printId() {
        println(id)
    }
}
```

直接调用即可

```kotlin
val contact = Contact(1, "mary@gmail.com")
contact.printId()
```

Data
数据类
`println()` 和 `print()` 会自动调用`toString()`

```kotlin
data class User(val name: String, val id: Int)  
  
fun main() {  
    val user = User("Alex", 1)  
    println(user)  
}
```

比较
`equals()` 或 `==`

```kotlin
val user = User("Alex", 1)
val secondUser = User("Alex", 1)
val thirdUser = User("Max", 2)

// Compares user to second user
println("user == secondUser: ${user == secondUser}") 
// user == secondUser: true

// Compares user to third user
println("user == thirdUser: ${user == thirdUser}")   
// user == thirdUser: false
```

https://kotlinlang.org/docs/kotlin-tour-classes.html#compare-instances

复制实例

```kotlin
data class User(val name: String, val id: Int)

fun main() {
    val user = User("Alex", 1)

    // Creates an exact copy of user
    println(user.copy())       
    // User(name=Alex, id=1)

    // Creates a copy of user with name: "Max"
    println(user.copy("Max"))  
    // User(name=Max, id=1)

    // Creates a copy of user with id: 3
    println(user.copy(id = 3)) 
    // User(name=Alex, id=3)

}
```
https://kotlinlang.org/docs/kotlin-tour-classes.html#copy-instance

