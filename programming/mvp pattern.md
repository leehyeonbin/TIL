# MVP Pattern
## MVP (Model - View - Preisenter) Pattern 이란?
![mvp_pattern]()
* MVP 패턴은 MVC(Model - View - Controller)패턴의 파생 패턴으로, 사용자 인터페이스를 개발하기 위해 대부분 사용된다.
* MVP에서 프리젠터는 "middle-man"의 기능을 담당한다. MVP에서 모든 프레젠테이션 로직은ㅇ 프리젠터로 넘어간다.

## MVP Pattern가 나오게 된 이유
    MVP Pattern이 나오게 된 이유는 View와 Model을 완전한 분리해서 사용하기 위해서이다.
MVP는 Model의 역할인 비즈니스 로직을 독립적으로 테스트할 수 있습니다.

## MVP 패턴의 구성 요소
MVP 모델은 Model - View - Presenter로 구성된다.

### 뷰(View)
실제 view 에 대한 직접적인 접근을 담당한다. 안드로이드에서 Activity/Fragment는 View의 일부로 정의 한다. View에서 발생하는 이벤트는 직접 핸들링 할 수 있으나 Presenter 에 위임하도록 한다. 위임하는 방법은 Activity 가 View interface 를 구현해서 Presenter 에서 코드를 만들 interface 를 갖도록 하면 된다. 이렇게 하면 특정 뷰와 결합되지 않고 가상 뷰를 구현해서 간단한 유닛 테스트를 실행할 수 있습니다.

### 프리젠터(Presenter)
본질적으로 MVC의 컨트롤러와 같지만, 뷰에 연결되는 것이 아니라 인터페이스로 연결된다는 점이 다르다. 이에 따라 MVC가 가진 테스트 가능성 문제와 함께 모듈화/유연성 문제 역시 해결된다. **프리젠터(Persenter)의 역할을 한줄로 표현한다면** 뷰(View)와 모델(Model) 사이에서 자료 전달 역할을 한다.

### 모델(Model)
앱 데이터 및 상태에 대한 비지니스 로직을 수행합니다.


출처 : [MVP(Model-View_Presenter)패턴](https://faith-developer.tistory.com/71)