# 拓展函数
## 为字符串类型添加一个拓展函数

```kotlin
fun String.bold(): String = "<b>$this</b>"

fun main() {
    // "hello" is the receiver object
    println("hello".bold())
    // <b>hello</b>
}
```
https://kotlinlang.org/docs/kotlin-tour-intermediate-extension-functions.html#extension-functions


```kotlin
class HttpClient { fun request(method: String, url: String, headers: Map<String, String>): HttpResponse { // Network code } }


fun HttpClient.get(url: String): HttpResponse = request("GET", url, emptyMap()) 
fun HttpClient.post(url: String): HttpResponse = request("POST", url, emptyMap())
```
https://kotlinlang.org/docs/kotlin-tour-intermediate-extension-functions.html#extension-oriented-design
