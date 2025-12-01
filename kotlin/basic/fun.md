## 需要为其指明参数数据类型，返回值类型

```kotlin
fun sum(a: Int, b: Int): Int {  
    return a + b  
}
```
## 单行函数

```
fun sum(x: Int, y: Int) = x + y
```
### `{}`声明函数体需指定返回值类型
### `Unit`在此无需指定

## 命名参数
### 在传参时加入形参

```kotlin
sum(a = 1, b = 2)
```
## 默认值
### 在创建函数时声明默认值

```kotlin
fun abc(a: Int = 20): Int {  
    return a  
}
```

## Lambda

```kotlin
val circleArea = { radius: Int -> PI * radius * radius }
val upperCaseString = { text: String -> text.uppercase() }
```

`->`之前是参数
`->`之后是函数体


```kotlin
val upperCaseString: (String) -> String = { text -> text.uppercase() }
```

```kotlin
(String) -> String
```

-> 之前是参数类型
-> 之后是返回值类型

无参数时为空

```kotlin
() -> Unit
```
https://kotlinlang.org/docs/kotlin-tour-functions.html#return-from-a-function


## Invoke separately
### 分别调用

Lambda 表达式可以通过在花括号 {} 后添加圆括号 () 并将任何参数包含在圆括号内来单独调用。
```kotlin
println({ text: String -> text.uppercase() }("hello"))
```

https://kotlinlang.org/docs/kotlin-tour-functions.html#invoke-separately


## Trailing lambdas ﻿[link](https://kotlinlang.org/docs/kotlin-tour-functions.html#trailing-lambdas)
### 尾随 lambda

如您所见，如果 lambda 表达式是函数的唯一参数，则可以省略函数括号 ()。如果 lambda 表达式作为函数的最后一个参数传递，则可以将其写在函数括号 () 之外。在这两种情况下，这种语法都称为尾随 lambda 表达式。
例如，.fold() 函数接受一个初始值和一个操作：

```kotlin
fun main() {
    // The initial value is zero. 
    // The operation sums the initial value with every item in the list cumulatively.
    println(listOf(1, 2, 3).fold(0, { x, item -> x + item })) // 6

    // Alternatively, in the form of a trailing lambda
    println(listOf(1, 2, 3).fold(0) { x, item -> x + item })  // 6

}
```

https://kotlinlang.org/docs/kotlin-tour-functions.html#trailing-lambdas