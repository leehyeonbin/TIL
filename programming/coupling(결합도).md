# 결합도(Coupling)
## 결합도란 
* 다른 모듈에 의존하는 정도를 나타내는 것이다. [소프트웨어 공학](https://ko.wikipedia.org/wiki/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4_%EA%B3%B5%ED%95%99)
* 모듈 간에 상호 의존하는 정도 또는 두 모듈 사이의 연결 관계를 의미한다.(2020 시나공 정보처리기사 실기)

간단하게 자바의 클래스로 예를 들면, 결합도가 높은 클래스는 다른 클래스와 연관된 정도가 높다. 따라서 해당 클래스를 변경하면 연관된 클래스도 변경해야 하며, 다른 코드에서 클래스를 재사용하기도 어렵다.

결합도는 아래와 같이 결합 정도에 따라 6개 단계로 구분된다.

## 자료 결합도(Data Coupling)
**가장 낮은 결합도를 갖는다. 가장 좋은 형태다.** 모듈끼리 단순히 파라미터 등을 통해 데이터를 주고받는 경우다.

여기서 주고받는 데이터는 모듈의 기능 수행에 있어서 로직을 제어하거나 하지 않는 순수한 자료형 요소이다. 또한 한 모듈을 변경하더라도 다른 모듈에는 영향을 끼치지 않는 결합 상태이다.

![data coupling](../img/data_coupling_img.jpg)

자료 결합도(Data Coupling)의 코드 예제를 살펴보자. 아래와 같이 모듈의 단위를 메서드로 정의했을 때, 다른 메서드에 단순 데이터 타입을 전달한다.

```java
public void main() {
    int result = plusDemo(1,2)
}

// 데이터를 전달한다.

public int plusDemo(int x, int y) {
    return x + y;
}
```


```kotlin
fun main() {
    plusDemo(3,4)
}

fun plusDemo(x : Int, y : Int) {
    return x + y
}
```

