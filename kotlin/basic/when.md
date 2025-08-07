```kotlin
val a = "HH"  
when (a) {  
    "H" -> println("Hi")  
    "HH" -> println("Hii")  
    else -> println("nothing")  
}
```

```kotlin
val button = "A"  
println(  
    when (button) {  
        "A" -> "Yes"  
        "B" -> "No"  
        "X" -> "Menu"  
        "Y" -> "Nothing"  
        else -> "There is no such button"  
    }  
)
```

只有第一个合适的分支会被执行。