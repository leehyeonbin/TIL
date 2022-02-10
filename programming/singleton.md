# 싱글톤
## 싱글톤 패턴이란?
싱글톤 패턴이란 객체의 인스턴스가 오직 1개만 생성되는 패턴을 의미한다.

* 어떤 클래스의 인스턴스는 **오직 하나**임을 보장하며, 이 인스턴스에 접근할 수 있는 전역적인 접촉점을 제공하는 **디자인 패턴**이다.
* 애플리케이션의 시작부터 종료까지 한 번의 생성으로 고정된 메모리 영역을 가지기 때문에 **메모리를 효율적으로** 사용할 수 있다.
* 싱글턴의 인스턴스는 전역적으로 사용되기 때문에 다른 클래스의 인스턴스들이 **데이터를 공유 변경**이 가능하는 점에 장점이 있다.

    -> 한 번의 선언으로 모든 클래스에서 전역으로 사용 가능한 필드나 메서드를 만드는 것 

```java
java

public class Singleton {
    private static Singleton instance = null;

    private Singleton() {
        // 생성자는 외부에서 호출하지 못하도록 private 로 지정해야 한다.
    }

    public static synchronized Singleton
    getInstance() {
        if (instance == null) {
            instance == new Signletone()
        }
        return instance;
    }
}
```

[synchronized란?](https://github.com/leehyeonbin/TIL/blob/main/programming/synchronized%EB%9E%80.md)

```kotlin
kotlin

object Singleton
```

정적 메서드란?
* static으로 선언한다.
* 클래스의 인스턴스 없이 호출이 가능하며, 인스턴스에서는 호출이 불가능하다.
* 유틸리티 함수를 만드는데 유용하게 사용된다.

object를 쓴 예
```kotlin
object GithubService {
    private const val BASE_URL = "https://api.github.com"

    private val retrofit:Retrofit = Retrofit.Builder()
        .baseUrl(BASE_URL)
        .addConverterFactory(GsonConverterFactory.create())
        .build()

    val service:GithubService = retrofit.create(GithubService::class.java)
}
```



출처 : [[안드로이드/kotlin] 싱글톤 패턴 사용해보기](https://gaybee.tistory.com/16)

자바와 동일한 방식으로 사용하는 방법 : **Companion object**

```kotlin
class BBQ{
    companion object {
        const val chicken ="delicious"

        fun wing() {
            // Do something
        }
    }
}
```

companion object 내에 선언된 속성과 함수는 (클래스 이름).(필드/함수 이름) 형태로 바로 호출이 가능하다.

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Something

    // BBQ.chicken() 함수 호출
    BBQ.chicken()
}
```

**companion object는 클래스 내부에서 선언되고, object 클래스 외부에서 선언된다.**