## 只读列表

```kotlin
val a = listOf(1,2,3,4,5)
```

## 明确类型可变列表

```kotlin
val b : MutableList <String> = mutableListOf("one","two","three","four","five","six")  
b.add("seven")  
println(b)
```
## 访问

```kotlin
println(b.first())  //第一个
println(b.last())   //最后一个
```
## 统计个数

```kotlin
println(b.count())
```
## 检查是否在内

```kotlin
println("one" in b) //返回布尔值
```
## 增删

```kotlin
b.add("seven") 添加
b.remove("two") //删除
```
