### `python`中叫做字典
### 键值对

```kotlin
val a = mapOf("aa" to 1, "bb" to 2)  
println(a["aa"])
```

### 明确类型的可变map

```kotlin
val b : MutableMap <String, Int> = mutableMapOf("aa" to 1, "bb" to 2, "ccc" to 3, "ddd" to 4);
```

### 可使用`[]`添加

```kotlin
b["num"] = 21
```

### 是否存在

```
println(b.contains("aa"))
```
### 键与值

```
println(b.keys)  
println(b.values)
```
