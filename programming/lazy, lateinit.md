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
## lazy
```kotlin
fun main() {
    lateinit var text : String
    val textLength : Int by lazy {
        text.length
    }

    // 대충 또 중간에서 뭐하고
    text = "Hello_Kotlin")
    println(textLength)
}
```

by lazy 구문으로 어떤 생성자를 넣어준 모양으로 보인다. 그런데 다시 봐보면 lateinit 이라서 아직 초기화가 되지 않은 text 의 속성을 활용한 모습을 볼 수 있다.

by lazy 는 선언 당시에서는 초기화를 할 방도가 없지만, 의존하는 값들이 초기화된 이후에 값을 채워 넣고 싶을 때 사용한다. 즉 **호출시에 이를 어떻게 초기화 해줄지에 대하여 정의 할 수 있는 구문이다** 
따라서 text가 초기화 된 이후에 textLength를 출력 할 때 text.length 속성을 사용하여 textLangth 변수를 초기화 한다.

그리고 선언부를 자세히 보면 val로 되어있는 것을 볼 수 있는데. **이 말은 즉슨 단 한 번의 늦은 초기화 이후 값이 불변한다는 것을 나타낸다!**
> ## Tip! 
> ### 안드로이드에서 이전 액티비티에서 넘어온 Intent Bundle Extra 등을 현재 액티비티 멤버 변수에 by lazy 로 받아와, 선언해 두고 사용시에 intent.extra 등으로 번들을 사용하면, 생명주기를 위반하지 않고 안전하게 클래스 전역에서 사용할 수 있는 값을 갖고올 수 있다!!

## 비교하기 
### **기본적으로 둘 다 본질적인 목적으로 늦은 초기화를 하기 위함은 비슷하다.**
### 하지만 **가변성에 관한 특성**이 갈리며, 이에 따라 용법을 구분하여 사용한다.

| 비교 | lateinit | lazy |
| -------| -------| ------- |
|가변성|가능(var 사용)|불가능(val 사용)|
|용법 | 초기화 이후에 계속 하여 값이 바뀔 수 있을 때 | 초기화 이후에 읽기 전용으로 값을 사용할 때|





출처 : 
* [[Kotlin] 🤚🏻 lateinit vs lazy, 정확히 아세요?](https://velog.io/@haero_kim/Kotlin-lateinit-vs-lazy-%EC%A0%95%ED%99%95%ED%9E%88-%EC%95%84%EC%84%B8%EC%9A%94)