带有接收者的 Lambda 表达式

```kotlin
class A1 {  
    fun test() = println("this is testing")  
}  
  
fun B1(pl: A1.() -> Unit) {  
    val a1 = A1()  
    a1.pl()  
}  
  
fun main(){  
    B1 {  
        test()  
    }  
}
```

个人总结，相当于给类换了个名字
可在B1函数体内调用类A1内容

当你想要创建领域特定语言 (DSL) 时，带有接收者的 Lambda 表达式非常有用。由于你无需显式引用接收者即可访问接收者对象的成员函数和属性，因此你的代码会变得更精简。
https://kotlinlang.org/docs/kotlin-tour-intermediate-lambdas-receiver.html#lambda-expressions-with-receiver