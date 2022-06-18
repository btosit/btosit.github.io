---
layout: default
title: Swift와 함수형 프로그래밍
parent: Swift
grand_parent: iOS
permalink: ios/swift/lecture1
---

# Swift와 함수형 프로그래밍
{: .no_toc }

## Agenda
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 함수형 프로그래밍이란?
시작하며
{: .label .label-purple }

Objective C와 Swift의 가장 큰 차이가 무엇이냐고 묻는다면 
둘다 객체지향 언어이지만 Swift는 함수형 언어라고 대답할 것 같다. 

함수형 언어가 무엇인가?
Swift에서는 함수가 **1급 객체**가 된다.
일급 객체(first-class object)란 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체를 가리킨다. 
함수의 인자(param)으로 함수를 넘기고, 함수의 결과로 함수를 리턴하고, 변수에 함수를 할당하는 연산을 지원하면 1급객체라고 할 수 있다.
Swift는 이것을 모두 지원하는 클로저를 직관적으로 사용할 수 있다는 특징이 있다.

여기서 클로저(Closure)란 이름없는 함수를 의미하는데
Swift에서 함수를 정의할 때 아래의 경우엔 
인자로 int a, int b를 전달받는 **sum**이라는 이름있는 함수를 정의한 것이 된다.  

```swift 
func sum(int a, int b) -> Int {
    return a + b
}
``` 

클로저는 아래와 같이 정의할 수 있다. 
```swift
let sum = { (a: Int, b: Int) -> Int in 
    return a + b
} 
```

그런데 그냥 심플하게 클로저는 곧 함수이고, 
그 함수에 이름이 있냐 없냐의 차이이기 때문에 굳이 둘을 구분할 필요는 없을 것 같다.


## 고차함수
 
고차함수(Higher order function)은 함수를 전달인자로 받거나 함수 실행의 결과를 함수로 반환하는 함수를 말한다.
Swift에서는 콜렉션에서 map, flatMap, filter, reduce 같은 고차 함수를 사용할 수 있다.
위 함수들은 모두 인자로 함수를 전달하도록 되어 있다.
어떤 역할을 하는지 어떻게 사용하는 함수인지 살펴보자.  
 
 
### 1. map

map은 시퀀스의 요소들에 대해 주어진 클로저를 맵핑한 결과들을 포함하는 배열을 리턴한다.

![](../../../assets/images/swift-lecture1-1.png)

numbers라는 int배열이 선언되어 있다고 하자.
이 콜렉션에서 사용할 수 있는 함수 중에 map을 볼 수 있는데
transform이라는 인자로 클로저를 전달해야하는 것을 볼 수 있다.

Example
{: .label .label-green }

```swift
let numbers = [1, 2, 3, 4, 5, 6, 7];
let results = numbers.map { number in
    return number + 3
}

print(results) // [4, 5, 6, 7, 8, 9, 10]
```

위의 경우 각 element에 대해 더하기 3을 한 값을 리턴하는 클로저를 전달했다.
최종적으로 results 배열은 numbers의 각 요소에 더하기 3된 값들의 배열이 된다.

### 2. flatMap

flatMap은 시퀀스의 요소들에 대해 새로운 시퀀스를 리턴하고 그것을 하나로 합쳐주는 역할을 한다.
  
![](../../../assets/images/swift-lecture1-2.png)

numbers라는 2차원 배열 [[Int]]이 선언되어 있다고 하자.
flatMap은 transform의 인자로
Int형 배열을 인자로 받아 Sequence를 리턴하는 클로저를 전달해야한다.

Example
{: .label .label-green }

```swift
let numbers = [[1, 2, 3], [4, 5, 6], [7]];
let results = numbers.flatMap { datas in
    return datas
}

print(numbers) // [[1, 2, 3], [4, 5, 6], [7]]
print(results) // [1, 2, 3, 4, 5, 6, 7]
```

[Int]을 datas라는 argument name으로 전달받은 뒤 그대로 datas를 리턴하는 클로저를 flatMap의 인자로 전달했다.
그러면 flatMap은 각 요소에 리턴된 시퀀스들을 하나로 합쳐주는 역할을 하기 때문에
이 경우 2차원 배열이 1차원 배열이 된다. 

### 3. filter
 
 주어진 predicate를 만족하는 시퀀스의 요소들을 포함하는 배열을 리턴한다.
 
 ![](../../../assets/images/swift-lecture1-3.png)
 
 Example
{: .label .label-green }
 
 ```swift
let numbers = [1, 2, 3, 4, 5, 6, 7];
let results = numbers.filter { number in
    return number > 3
}

print(numbers) // [1, 2, 3, 4, 5, 6, 7]
print(results) // [4, 5, 6, 7]
 ```
numbers배열에서 3보다 큰 element를 필터링한 배열이 최종적으로 리턴된다.

### 4. reduce

주어진 클로저를 이용해서 요소들을 결합한 결과를 최종적으로 리턴한다.

 ![](../../../assets/images/swift-lecture1-4.png)

reduce 메소드에 전달하는 클로저에서
첫번째 매개변수(initialResult)는 초기값을 의미하고,
두번째 매개변수 nextPartialResult는 다시 클로저를 전달해야하는데 
이 클로저의 첫번째 인자는 초기값에서 출발해서 현재까지 순회하는 동안의의 결과값을 의미하고,
두번쨰 인자는 현재 순회하고 있는 요소를 뜻한다.

Example
{: .label .label-green }

```swift
let numbers = [1, 2, 3, 4, 5, 6, 7];
let result = numbers.reduce(0, { (nextPartialResult, current) in
    return nextPartialResult + current
})
print(result) // 28
```

위의 경우에 초기값을 0으로 전달하고, 
nextPartialResult에는 초기값부터 시작해서 현재 순회하고 요소까지 올동안의 결과값이며 
current는 현재 순회하고 있는 요소가 된다.
nextPartialResult와 current의 합을 리턴하기 때문에 
결과적으로 numbers배열의 모든 요소들을 합한 결과가 최종적으로 리턴된다.

위의 코드는 아래와 같이 훨씬 더 심플하게 표현할 수 있다.
의미는 동일하게 초기값을 0으로 하고 축적된 결과값($0)과 현재 순회하고 있는 요소($1)를 더한 값을 리턴한다는 뜻이다. (return 생략 가능)   

```swift
numbers.reduce(0) { $0 + $1 }
```


## Swift의 특징

기본적으로 함수형 프로그래밍은 함수의 결과값이 함수의 인자에만 의존하도록 설계되기 때문에 외부의 영향을 최소화할 수 있다.
동일한 입력에 대해 다른 결과값이 리턴되는 경우가 없다는 것이다.


이외에 Swift에서는 별도의 헤더파일이 존재하지 않는다.
때문에 개인적으로는 파일 관리하기가 조금 더 용이한 것 같다.
문법도 Objective C에 비해 심플해서 같은 기능을 개발해도 Swift로 개발하면 생산성이 높다고 느껴진다. 
또, Optional Chaining으로 Nil Exception 을 방지 할 수도 있고, Force unwrapping을 이용해서 잘못된 데이터가 발생하는 경우에 개발자 입장에서 빨리 캐치하는데 도움이 된다. 그래서 나는 Objective-C보다는 Swift 개발을 선호하고, 이제 Objective-C 코드보는게 괴롭다.
