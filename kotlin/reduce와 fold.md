# Reduce 와 Fold
Kotlin 컬렉션에는 컬렉션의 데이터를 모두 모으는 함수인 reduce() 함수와 fold() 함수가 있다. **Reduce**와 **Fold**는 순서가 있는 데이터에 대해서 각 데이터에 대해 연산을 재귀적으로 해주는 함수이다. 여기서는 reduce와 fold 함수에 대한 설명과 각 함수간 차이를 살펴보자.

## Reduce
먼저 reduce에 코드를 살펴보면
```kotlin
public inline fun <S, T : S> Iterable<T>.reduce(
    operation: (S, T) -> S
): S
```
**초기 값**은 맨 처음의 값이며, **반환 데이터**는 맨 처음의 데이터 타입을 따른다는 것을 알 수 있다.

## 흐음...
아래 코드는 list 에 reduce 연산을 한다고 할 때, 이를 람다식으로 구현한 것이다. 아래 코드와 수행 순서를 보면 조금 아마 이해가 될 것이다.
```kotlin
val list = listOf(10, 4, 1)
val result = list.reduce { sum, num -> sum + num }

// result 는 15
```

위 코드를 수행하면 아래와 같은 연산 순서를 가진다.
```kotlin
1. 10 + 4 = 14
2. 14 + 1 = 15
3. 15 반환
```

## Fold
이번에는 Fold에 코드를 살펴보자면
```kotlin
public inline fun <T, R> Iterable<T>.fold(
    initial: R,
    operation: (R, T) -> R
): R
```
**초기 값**은 파라미터를 통해 자유롭게 지정해 줄 수 있고, **반환 값** 또한 파라미터를 통해 받아온 초기값의 자료형이 된다는 걸 알 수 있다.

## 이건..?
```kotlin
val list = listOf(10, 4, 1)
val result = list.fold(10){sum, num -> sum + num}

// result 는 25
```
이번에도 코드가 수행되는 연산 순서를 확인하자면
```kotlin
1. 10 + 10 = 20
2. 20 + 4 = 24
3. 24 + 1 = 25
4. 25 반환
```

## 차이점은?
앞에서 본 것 처럼 redece 는 초기값과 데이터 타입이 **가장 처음의 값**을 따르지만, fold 는 파라미터로 **받은 값**이 초기값이 되고, 데이터 타입이 이에 결정되기 때문에 사용하는 방법이 **조금 다르다.**

## 뭐가 다른데?
## 다른점(1)
redece와 fold는 상당히 유사해 보이지만, 만약 각 요소들에 2를 곱해서 계산한다고 하였을 때 reduce 에서는 다른 값이 나온다.

```kotlin
val numbers = listOf(1, 5, 2, 4, 6)

val sumUsingRedece = numbers.reduce { total, number -> total + number * 2}

println(sumUsingRedece) // 35(?)


val sumUsingFold = numbers.fold(0) {total, number -> total + number * 2} // 36

println(sumUsingFold)
```

## 의도한 값이 나오지 않는 까닭
> 위에서 redece 가 다른 값이 나온 까닭은 **reduce의 초기값은 맨 처음 값**이기 때문이다. 따라서 연산할 때 2를 곱하는 **number로 연산이 시작하는게 아닌** **total로 시작**하기 때문에 **2가 곱해지지 않아** 다른 값이 나온다.

-> 따라서 어떠한 연산을 할 때 첫 번째 값도 포함시켜야 한다면 fold를 사용해야 한다.

## 다른점(2)
만약에 비어있는 리스트를 연산한다면 어떤 값이 나올까?

```kotlin
val arr = EmptyList<Int>()

val emptyFold = arr.fold(10) {total, number -> total + number}
println("fold : $emptyFold") // 10

val emptyReduce = arr.reduce {total, number -> total + number}
println("reduce : $emptyReduce") // ?
```
이런 식으로 비어있는 리스트로 연산을 진행한다면 결과는
```kotlin
fold : 10

Empty collection can't be reduced.
java.lang.UnsupportedOperationException: Empty collection can't be reduced. ...
```
이처럼 fold 는 10을 넣어 값이 연산외 됬지만, reduce 는 오류가 난다. 따라서 컬렉션이 비어있을 경우 fold를 써야한다. 

### 참고
* [[Kotlin] reduce 와 fold](https://blog.leocat.kr/notes/2020/03/09/kotlin-reduce-and-fold)
* [[코틀린] reduce, fold 함수](https://velog.io/@blucky8649/%EC%BD%94%ED%8B%80%EB%A6%B0-reduce-fold-%ED%95%A8%EC%88%98)


