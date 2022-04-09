# 다형성(Polymorphism)
## 다형성(Polymorphism)이란?
**다형성 -> 다양한 성질 이라는 뜻**
<br>
여러 개의 형태를 갖는다는 의미로, 객체 지향 프로그래밍의 3대 특징 중 하나이다. 상속을 이용한 기술로, 자식 객체를 부모 클래스 타입의 변수로 다룰 수 있다. 다형성이란 같은 자료형에 여러 가지 객체를 대입하여 다양한 결과를 얻어내는 성질을 의미한다.

## 다형성 이란!
* 다형성이란 하나의 객체에 상속관계로 이뤄진 여러가지 type을 대입할 수 있는 것이다.
* 다형성은 두가지 타입이 있다.

1. compile time polymorphism(static binding) -> method overloading
2. runtime polymorphism(dynamic binding) -> method overriding

## Overloading & Overriding
* **Overloading** 은 같은 이름의 메서드가 매개변수만 달리하여 여러번 재정의 되는 것이다.
단! 리턴 타입은 보지 않는다.
 * static method를 overloading 한 경우, static method를 호출한 변수의 type 에 의해서 compile time 에 미리 호출될 method이 type이 정해진다.
 
```kotlin
class Calculation {
    fun add(x : Int, y : Int) : Int = x + y
    fun add(x : Double, y : Double) : Double = x + y
    fun add(x : Int, y : Int, z : Int) : Int = x + y + z
    fun add(x : String, y : String) : String = x + y
}
```

* **Overriding**이란 부모클래스의 메서드를 자식클래스에서 재정의(수정 or 추가)해서 사용 할 수 있는데 이걸 오버라이팅(Overriding)이라고 부른다.
* overrding method의 type은 runtime 시점에서 결정된다.

```kotlin
open class Human {
    open fun sing() = println("노래를 부른다.")
}

class Singer : Human() {
    override fun sing() = println("노래를 잘 부른다.")
}
```
용어가 헷갈릴 수 있으니 주의 하기
