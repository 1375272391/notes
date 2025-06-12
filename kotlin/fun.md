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

-> 之前是参数类型
-> 之后是返回值类型

无参数时为空

```kotlin
() -> Unit
```
https://kotlinlang.org/docs/kotlin-tour-functions.html#return-from-a-function

