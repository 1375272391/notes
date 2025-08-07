返回类型后加`?`

```kotlin
 // nullable has nullable String type
var nullable: String? = "You can keep a null here"
```

具备类型的变量为其赋值为空时将抛出错误

```kotlin
var neverNull: String = "This can't be null"

// Throws a compiler error
neverNull = null
```

未声明类型时可将空赋值

```kotlin
nullable = null
```

安全调用
为数据类型、参数后添加`?`

```kotlin
fun lengthString(maybeString: String?): Int? = maybeString?.length

fun main() { 
    val nullString: String? = null
    println(lengthString(nullString))
    // null
}
```

## Elvis 运算符
返回默认值

```kotlin
val nullString: String? = null  
println(nullString?.length ?: 0)
// 0
```
