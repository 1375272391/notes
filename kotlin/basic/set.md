# 集合
## 特性：无序、不重复

## 只读

```kotlin
val a = setOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

## 可变

```kotlin
val a = mutableSetOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
a.add(11)  
println(a)
```
### 可变带属性

```kotlin
val a: MutableSet<String> = mutableSetOf("sa")
```
增删、计数与列表一致
