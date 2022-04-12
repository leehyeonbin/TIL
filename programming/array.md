# Array
## 코틀린에서 배열은 Array로 클래스로 표현한다.
배열 기본 사용 방법 
* arrayOf( ) : 기본 생성자
* Array() : 기본 생성자
* arrayOfNulls() : 빈 상태의 배열
```kotlin
val numbers = arrayOf(1,2,3)
val animals = arrayOf("Cat", "Dog", "Lion")
```

다 차원 배열
```kotlin
val array1 = arrayOf(1,2,3)
val array2 = arrayOf(4,5,6)
val array3 = arrayOf(7,8,9)

// 1
val arr1 = arrayOf(array1,array2,array3)

//2
val arr2 = arrayOf(arrayOf(1,2,3), arrayOf(4,5,6), arrayOf(7,8,9))
```

2차원 배열도 가능하지만, 차원이 높아질수록 오류가 발생할 확률이 높음

### 배열에 타입 지정해주기

```kotlin
val array1 = arrayOf<String>("Hi","Hello","Good")
val array2 = arrayOf<Int>(1,2,3,4)
val array3 = arrayOf(1,"two",3.0,"4",true)
```

특정 자료형으로 제한 할 수 있지만, 만약 제한하지 않는다면 문자열, 정수, Boolean 등 여러가지 자료형을 혼합할 수 있음

## 배열에 값 확인, 넣기 
```kotlin
fun main() {
    val arr1 = arrayOf(1,2,3)

    println(arr1.get[0])
    println(arr1.get[1])
    println(arr1.get[2])

    arr1[0] = 100
    a.set(1, 200)

    println(arr1[0])
    println(arr1[1])
    println(arr1[2])
}
```

결과는
```kotlin
1
2
3
100
200
3
```
이 출력된다.

### 코드를 보면 알 수 있듯이 특정 인덱스의 엘리먼트를 리턴하여 인덱스 연산다고 호출할 수 있음

>### 특징 : Array의 경우 선언 당시에 크기가 정해진다.