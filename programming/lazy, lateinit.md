# lazy, lateinit 이란??
## kotlin 에서 늦은 초기화!
### 늦은 초기화라 함은, **말 그대로 초기화를 늦게 하는 것** 이다.<br>
### 예를 들어 a 라는 변수를 **분명 사용할 예정인데 a의 첫 상태를 정의하기 어려울 때** 어떻게 해야할까<br> 어떤 계산 값이 필요하다거나 하는 등의 상황에서 사용 될 것이다.
```kotlin
var a : Int? = null
```
### 이런 식으로 정리할 수 도 있지만, **나중에 무조건 사용 할건데,** 굳이 초기 상태를 null로 지정하는 것은 **null 사용의 지양을 강조**하는 코틀린에서 좋지 않다.
이런 상황에서 사용하는 것이 바로 lazy와 lateinit이다!!

## lateinit
### 일단 코드부터 살펴보자!!
```kotlin
fun main () {
    lateinit var text : String

    // 대충 중간에 뭐 했다고 치자 
    val result1 = 50

    text = "Result : $result1"
    println(text)

    // 대충 또 뭐 했다고 치자
    val result2 = 120

    text = "Result : ${result1 + result2}"
    println(text)
}
```

### lateinit 을 사용하여 text 변수를 선언해줬고, 이후에 어떤 동작의 결과를 기반으로 text를 초기화 해주는 것을 확인할 수 있다.<br>
이후에 또 한 번 값을 바꾸는 것을 알 수 있는데
>lateinit 변수 선언부를 자세히 보면 **var** 로 선언 되어있다. **lateinit을 사용하면 늦은 초기화 이후에도 값이 계속 바뀔 수 있다.**

그럼 만약 lateinit을 사용해놓고 늦은 초기화조차 하지 않은 경우는??
```
Exception in thread "main" kotlin.UninitializedPropertyAccessException: lateinit property text has not been initialized
```
이렇게 **컴파일 단계에서 오류가 발생**하기 때문에 **잠재적 오류를 방지**해준다!

>lateinit 은 값이 계속 변경될 수 있다는 속성을 위해 무조건 var을 사용해야 하며, **Primivie Type(Int, Float, Double, Long 등)에는 사용할 수 없다.**