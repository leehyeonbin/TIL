# filter 와 map
코틀린의 장점 중 하나는 자바에서는 아마 대부분 수작업으로 해야 했던 부분들을 기본적으로 제공해 준다는 것이다. 그 중에서도 **filter** 와 **map** 등의 함수들은  조금 더 코틀린스러운 코딩을 할 수 있도록 한다.
___

## Filter
### 먼저 예시로 리스트를 만들어 준다.
```kotlin
val list = {"chicken", "pizza", "pasta", "hambuger"}
```

## List\<String> 에서의 filter 사용
### filter
```kotlin
list.filter{
    it.startWith("p")
}
// ["pizza", "pasta"]
```
조건식에 따른 Boolean 에 따라 필터링의 여부를 결정한다. 위의 코드에서 조건식은 '값이 p로 시작하는가' 이다. 따라서 결과는 위의 주석과 필터링된다.

### filterNot
이에 반대로 filterNot 이라는 것도 있다. filterNot은 조건이 아닐 경우에 남긴다.
```kotlin
list.filterNot{
    it == "pizza"
} 
// ["chicken", "pasta", "hambuger"]
```

### filterIndexed 
인덱스틀 통해 값을 처리하는 경우에도 있다. 이러한 경우네는 filterIndexed를 사용하면 된다.

```kotlin
list.filterIndexed {
    it == 2
}
// ["pasta"]
```
---
## List<Any?>
List\<Any?> 에서 필터를 사용할 때 null 이 있는 경우에는 어떻게 해야할까?

```kotlin
val nullList = {"apple", null, "orange", "banana", "ade"}
```

### FilterNotNull
```kotlin
nullList.filter {
    it.startWith("p")
}
```
위와 같은 코드는 NotNullString 에서 사용하는 것이기 때문에 컴파일이 되지 않을 것이다. 이러한 경우에는 그저
```kotlin
it?.startWith("p") ?: false 
``` 
이런식으로 사용하여도 크게 상관없다. 하지만 filter를 이용해서 해결하는 방법도 있다.

```kotlin
nullList.filterNotNull().filter {
    it.startWith("a")
}
// ["apple", "ade"]
```

### FilterIsInstance
```kotlin 
val anyList = {"banana", "chicken", 2, "mango", 52, null}
```
이런식으로 안에 Int가 포함이 되어있는 Any 타입의 List 또한 String에서만 사용하는 함수를 사용할 수 없다. 이경우에는 필터안에서 if문을 이용하여 이를 해결 할 수 있지만, 값이 들어올때마다 if 문이 돌아가는 것은 번거로운 일일테니 filter를 사용하여 해결할 수 있다.

```kotlin
anyList.filterIsInstance<String>().filter {
    it.startWith("c")
}
// ["chicken"]
```
---
## Map
### 연산
map 함수는 각 원소에 원하는 형태(연산)로 변환하는 기능을 하고, 변환된 값들을 모아서 컬렉션을 새로 만든다. 결과는 처음 리스트와 원소 개수는 같지만, 각 원소는 주어진 함수에 따라 새로운 컬렉션이 된다.
```kotlin
val list = [1 ,2 ,3 ,4]
val afterList = list.map { it * it }
// [1 ,4 ,9 ,16]
```

### 변환
```kotlin
val personList = listOf(Person("코틀린", 18), Person("안드로이드", 20))
println(personList.map {it.name})
// [안드로이드, 코틀린]
```

```kotlin
personList.map(Person::name) // 이런식으로 멤버 참조 문법도 가능하고
```

이런식으로 함수를을 사용할 경우에 예를 들면 나이가 20이상인 사람의 이름만 가져오기
```kotlin
personList.filter{it.age >= 20}.map(Person::name)
// ["안드로이드"]
```

이런식으로 멋진 코드들이 가능하다.

### 괜찮은 코드
나이가 가장 많은 사람을 불러오는 코드를 작성해보자
```kotlin
personList.filter{ it.age == personList.maxBy(Person::age)!!.age } 
// 내부에서 MaxBy를 이용하는 비효율적인 코드이다.

val maxAge = personList.maxBy(Person::age)!!.age
personList.filter {it.age == maxAge } // 외부에서 maxBy를 사용하는 비교적 효율적인 코드이다.
```

위에 코드를 보면 첫 번째 코드에는 maxBy를 **내부**에서 실행하여 연산을 모든 값들이 실행되는 동안 계속 실행해야 한다. 하지만 두 번째 코드는 maxBy를 **외부**에서 사용하여 1번만 사용하고, 그 값에 필터링을 한다.

> 여기서 알 수있는 점은 무작정 코드를 줄인다고 해서 다 좋은 것이 아니라는 것이다. 어떤식으로 코드가 구동되는지 잘 확인하고 작성해아한다.


## 결론
>filter 를 굳이 이용하지 않아도 얼마든지 구현이 가능하고, 항상 그렇게 했었지만, 코드가 길어지고 커질수록 코드가 복잡하고 이해하기 어려뤄지니 Kotlin에서는 코틀린다운 편리한 기능들을 알아서 코딩하는 것이 좋을 것 같다.

### 출처
* [[Kotlin] 코틀린 Collection 에서 사용하는 Filter 함수
](https://blog.yena.io/studynote/2020/01/22/Kotlin-Collection-Filter.html)
* [코틀린(Kotlin) - 컬렉션 API : filter, map, all, any, count, find, groupBy, flatMap, flatten](https://0391kjy.tistory.com/38)