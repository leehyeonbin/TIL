# synchronized 란?
synchronized 란 한국어로 직역하면 **동기화** 라는 뜻이다.

    둘 이상의 쓰레드가 공동의 자원(파일이나 메모리 블록)을 공유하는 경우, 순서를 잘 맞추어 다른 쓰레드가 자원을 사용하고 있는 동안 한 쓰레드가 절대 자원을 변경할 수 없도록 해야 한다. 한 쓰레드가 파일에서 레코드를 수정하는데, 다른 쓰레드가 동시에 같은 레코드를 수정하면 심각한 문제가 발생할 수 있다. 이런 상황을 처리할 수 있는 한 방법은 관련된 쓰레드에 대한 동기화(synchronized)를 이용하는 것이다.

동기화의 목적은 여러 개의 쓰레드가 하나의 자원에 접근하려 할 때 주어진 순간에는 오직 하나의 쓰레드만이 접근 가능하도록 하는 것이다. 
동기화를 이용해 쓰레드의 실행을 관리할 수 있는 방법은 두 가지가 있다.

* 코드를 메소드 수준에서 관리할 수 있다. -동기화 메소드
* 코들르 블록 수준에서 관리할 수 있다. - 동기화 블록

동기화 메소드와 동기화 블록스 모두 **synchronized** 를 이용하여서 구현된다.

synchronized 는 어떤 객체에서도 특정 문장 블록에 대해 lock 설절을 할 수있다. 사용 방법은 다음과 같다.
```java
synchronized( theObject )
    statment; // theObject가 동기화 된다.

synchronized( theObject) {
    statement; // theObject가 동기화 된다.
}
```
statement 문장이 실행되는 동안 theObject 는 동기화된 다른 쓰레드에서 사용할 수 없다. 동기화된 다른 쓰레드라는 것은 위의 코드와 동일하게 synchronized 를 사용하였거나 synchronized method 를 사용한 쓰레드를 의미한다.

synchronized method 의 예제는 아래와 같다.

```java
class theObject {
    synchronized public void method() {
        statement;
    }
}
```

출처 : [66. synchronized 키워드의 의미를 알고 메소드와 블록에 붙을 때의 차이를 알아야 한다.](https://opentutorials.org/module/1226/8028)

