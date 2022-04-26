# Dagger2
## Dagger2란?
Dagger2는 자바와 안드로이드를 위한 강력하고 빠른 의존성 주입 프레임워크다. 리플렉션을 사용하지 않고, 런 타임에 바이트 코드도 생성하지 않는 것이 특징이다. 컴파일 타임에 애노테이션 프로세서에 의해 의존성 주입과 관련된 모든 코드를 분석하고 자바 소스 코드를 생성한다.

## Dagger2 의 장점
* 자원 공유의 단순화. 지정된 범위의 생명 주기 내에서 동일 인스턴스를 제공한다.
* 복잡한 의존성을 단순하게 설정함. 애플리케이션이 커질수록 많은 의존성을 갖는데 Dagger는 이를 쉽게 제어해 준다.
* 유닛 테스트를 쉽게 도와준다.
* 자동 코드 생성. 생성된 코드는 명확하고 디버깅이 가능하다.
* Dagger2는 난독화 문제가 없다. (Dagger1은 리플렉션의 사용으로 인해 런탕미에 성능 및 난독화와 관련된 문제가 발생했다.)
* 라이브러리 크기가 작다.

## Dagger 설정하기
~~~gradle
dependencies {
    implementation 'com.google.dagger:dagger-android:$Version'
    implementation 'com.google.dagger:dagger-android-support:$Version'
    annotationProcessor 'com.google.dagger:dagger-android-processor:$Version'
    annotationProcessor 'com.google.dagger:dagger-compiler:$Version'
}
~~~
코틀린 언어세ㅓ 사용하는 프롲게트는 다음과 같이 annotationProcessor 를 kapt로 변경해야 한다.

~~~gradle
dependencies {
    implementation 'com.google.dagger:dagger-android:$Version'
    implementation 'com.google.dagger:dagger-android-support:$Version'
    kapt 'com.google.dagger:dagger-android-processor:$Version'
    kapt 'com.google.dagger:dagger-compiler:$Version'
}
~~~

최신 버전을 권장하고, 최신 버전과 자세한 정보는 [링크](https://github.com/google/dagger)를 참고하길 바란다.

# 구현하기
Dagger 의 설정을 끝낸 후 "Hello World" 문자열을 제공한 모듈을 만드는 예제를 해보겠다.
~~~kotlin
@Module
class MyModule {
    @Provides
    fun provideHelloWorld() = return "Hello World"
}
~~~

앞에서 언급했던 것 처럼 Dagger는 컴파일 타임의 의존성 주입에 필요한 애노테이션을 읽고 의존성 주입에 필요한 파일을 생성한다. **@Module은 의존성을 제공하는 클래스에 붙이고, @Provides 애노테이션을 읽고 의존성 주입에 필요한 파일들을 생성**한다. 

MyModule 클래스 하나만으로는 별도의 클래스 파일이 생성되지 않는다. 모듈을 참고하는 컴포넌트가 없기 때문이다.
~~~kotlin
@Component(modules = MyModule.class)
interface MyComponent {
    getString() : String //  프로비전 메서드, 바인드된 모듈로부터 의존성 제공
}
~~~
>@Component가 붙은 MyComponent 인터페이서 내에는 제공할 의존성들을 메서드로 정의해야 하며, @Component에 참고된 모듈 클래스로부터 의존성을 제공 받는다.

 >-> 메서드의 반환형을 보고 모듈과 관계를 맺기 때문에 바인드된 모듈로부터 해당 반환형을 갖는 메서드를 찾지 못하면 컴파일 타임에 에러가 발생한다.

Dagger 는 컴파일 타임에 @Component를 구현한 클래스를 생성하는데, 이때 클래스의 이름은 'Dagger'라는 접두사가 붙는다. 예를 들어  MyComponent라는 컴포넌트를 만든다면, DaggerMyComponent 라는 이름으로 만들어진다.

TextView로 실행하도 되지만, JUnit 테스트 클래스 작성을 통해 의존성이 제대로 제공되는지 확인해볼 수 있다.

~~~kotlin
class ExampleUnitText {
    @Test
    fun testHelloWorld() {
        val myComponent : MyComponent = DaggerMyComponent.create()
        println("result : ${myComponent.getString()}")
    }
}
~~~
이런식으로 코드를 작성한 후 
~~~kotlin
result = HelloWorld
~~~
이런식으로 출력이 될 것이다!

~~사실 책에 나온 자바 코드를 나름대로 코틀린으로 변환한거라 틀릴 수도 있다.~~
아무튼 이런식으로 간단하게 의존성 주입의 구현을 성공한 것이다. 컴포넌트와 모듈만 사용하여 아주 간단하게 예제를 만들어 보았다.