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


## 싱글톤 패턴을 사용하는 이유
* 고정도니 메모리 영역을 얻으면서 한번의 new로 인스턴스를 사용하기 때문에 메모리 낭비를 방지할 수 있다.
* 싱글톤으로 만들어진 클래스의 인스턴스는 전역 인스턴스이기 때문에 다른 클래스의 인스턴스와 데이터를 공유하기 쉽다. 
* 안드로이드 앱 같은 경우 각 액티비티나 클래스별로 주요 클래스들을 일일이 전달하기가 번거롭기 때문에 싱글톤 클래스를 만들어 어디서나 접근하도록 설계하는 것이 편하다.

## 싱글톤 패턴의 문제점
* 싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유 시킬 경우에 다른 클래스의 인스턴스들 간에 결합도가 높아져 "개방-폐쇄 원칙"을 위배하게 된다.

이는 객체 지향 설계 원칙에 어긋나기 때문에 수정이 어려워지고 유지보수의 비용이 높아질 수 있다. 또한 멀티쓰레드 환경에서 동기화 처리를 안하면 인스턴스가 2개가 생성 될 수 있는 가능성이 생기게 된다.

**이러한 이유로 싱글톤 패턴은 꼭 필요한 경우가 아니라면 지양해야 한다.**