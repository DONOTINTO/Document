# ARC(Automatic Reference Counting)란?
swift는 ARC(Automatic Reference Counting)를 통해 앱의 메모리를 관리한다. ARC는 어떤 클래스의 인스턴스가 필요하지 않을 때 자동으로 참조 카운트를 체크하여 인스턴스에 할당된 메모리를 해제하여 관리하는 시스템이다.

우선 모르는 단어가 투성이다. 인스턴스, 참조, 메모리 등...

---

## 메모리 영역
우선은 프로그램의 메모리 영역에 이해가 필요할 것이다.
프로그램이 실행되면 RAM에서 해당 프로그램의 메모리가 할당되어 사용할 수 있게 된다.
그렇게 할당받은 메모리는 총 4개의 영역으로 구분된다.

![](https://velog.velcdn.com/images/donotinto/post/24b3fca5-a66e-4ac3-8274-30a91b447765/image.png)

- **코드(Code)** - 코드가 입력되는 부분
- **데이터(Data)** - 전역 변수
- **힙(Heap)** - 동적할당변수
- **스택(Stack)** - 지역 변수, 파라미터

코드와 데이터는 메모리의 가장 낮은 부분부터 메모리를 채운다. 그리고 스택은 가장 높은 부분부터 쌓여 나간다. 그 외 나머지 중간 부분을 Heap이 동적으로 관리한다.

이번 ARC에서 알아야 하는 부분은 Heap영역으로 동적할당변수에 해당한다.

동적으로 관리한다는 것은 메모리의 영역이 커졌다 작아졌다 한다는 의미로, 이 Heap영역을 어떻게 관리하냐가 메모리 관리에 중요한 포인트일 것 이다.
~~(메모리 관리를 못해 할당된 메모리 이상 사용하게 되면 프로그램이 터지기 때문이다)~~

그러면 Heap영역에서 관리하는 것은 동적할당변수에 해당하는 것은 무엇일까?

---

## 동적할당변수

swift에는 값타입과 참조타입라는 두가지 타입이 존재하는데, 그 중 참조타입이 Heap영역에서 관리되는 동적할당변수이다.

그리고 참조타입에서 가장 대표적인 것이 바로 **클래스(Class)** 이다.
(값 타입 - Sturct / Enum 등)


클래스로 객체를 생성하면 Heap영역에 메모리를 할당받는데, 이때 생성된 객체를 **인스턴스** 라 부른다. 

우리는 이제 ARC는 클래스로 Heap영역에 생성된 객체인 인스턴스에 메모리 관리 시스템이라는 것을 이해할 수 있게되었다..
그러면 참조는 무엇일까?

---

## 참조(Reference)

```swift
var a = 1
var b = a

b = 2
print(a)
//1
```
위 코드는 **값 타입**의 예시이다.
b는 a의 값을 복제하였지만 b의 값을 변경한다 하여 a의 값이 변하지는 않는다.

이를 통해 값 타입은 개별적으로 동작하는 것을 알 수 있는데, b를 a로 복제하였다 하더라도 b는 a의 **'값'** 만을 복제한 것이기 때문이다.

```swift
class Person {
	var age = 20
}

var a = Person()
var b = a

b.age = 10
print(a.age)
// 10
```
위 코드는 **참조 타입** 의 예시이다.
이번에도 마찬가지로 b는 a를 복제하였지만, b의 프로퍼티를 변경하자 a의 프로퍼티도 변경된 것을 볼 수 있다.

a는 클래스 Person의 인스턴스를 생성하였고, b는 이 인스턴스 주소값을 복제하였기 때문이다. 

a라는 변수는 스택 영역에 생성되지만 Person 인스턴스는 Heap영역 생성되어 a는 Person인스턴스의 메모리 주소를 저장하고 있을 뿐이다.

![](https://velog.velcdn.com/images/donotinto/post/f3cd7b29-c8e6-4ec3-be09-d05774eb9476/image.png)

이때 b 역시 스택에 생성되지만 a가 가지고 있는 Person 메모리 주소를 가지게 된다.

![](https://velog.velcdn.com/images/donotinto/post/4dc64602-95b6-4302-b0bd-d650f5c51ad8/image.png)

이처럼 a와 b는 Person 인스턴스를 **참조**하고 있다고 말할 수 있다.

## ARC
클래스의 객체(인스턴스)가 생성되면 ARC는 해당 인스턴스에 대한 정보를 저장하기 위한 메모리를 할당하고, 인스턴스를 참조하는 프로퍼티, 상수, 변수의 수를 카운팅한다.

즉, 레퍼런스 카운팅이 1 이상이라면 어디선가 해당 인스턴스를 참조하고 있다는 의미이므로 ARC는 인스턴스의 메모리를 유지한다. 그러나 레퍼런스 카운팅이 0이라면 메모리를 해제하여 메모리를 확보한다.

```swift
class Character {
    var nickname: String
    
    init(nickname: String) {
        print("Character 메모리 할당")
        self.nickname = nickname
    } 
    deinit {
        print("Character 메모리 해제")
    }
}

var a: Character? = Character(nickname: "홍길동") // reference count : 1
var b: Character? = a // reference count : 2

a = nil // reference count : 1
//output
//Character 메모리 할당
```

위 예시의 경우 
```swift
var a: Character? = Character(nickname: "홍길동")
```
해당 코드가 실행될 때 ARC는 Character 인스턴스에 대한 메모리를 할당하고, 변수 a가 Character 인스턴스를 참조하기 때문에 reference count가 1 증가한다.

b 역시 a가 가지고 있는 Character 인스턴스를 참조하여 reference count가 2가 된다.

이 후 a를 nil로 변경해주어도 b가 아직 해당 인스턴스를 참조하고 있기 때문에 reference count는 1로 메모리가 해제되지 않는다.

```swift
class Character {
    var nickname: String
    
    init(nickname: String) {
        print("Character 메모리 할당")
        self.nickname = nickname
    } 
    deinit {
        print("Character 메모리 해제")
    }
}

var a: Character? = Character(nickname: "홍길동") // reference count : 1

a = nil // reference count : 0
//output
//Character 메모리 할당
//Character 메모리 해제
```
위 경우에는 reference count가 0이 되어 ARC를 통해 메모리가 해제되는 것을 확인할 수 있다.

---
물론 이렇게 ARC를 통해 모든 참조 카운트가 되어 메모리 관리가 된다면 좋지만, 간혹 서로의 프로퍼티가 서로를 참조하는 경우 ARC가 작동하지 않아 메모리 누수가 발생할 수 있다.

![](https://velog.velcdn.com/images/donotinto/post/80d2706b-9800-4a3a-aa9e-c9ea7e8b0fb4/image.png)

- 변수 a는 Chracter 인스턴스를 참조
- 변수 b는 Item 인스턴스를 참조
- a의 프로퍼티 item은 b를 참조
- b의 프로퍼티 owner는 a를 참조

위와 같을 때
a와 b를 nil로 인스턴스를 없애주어도,
a의 프로퍼티 item과 b의 프로퍼티 owner가 서로를 아직 참조하고 있기 때문에 reference count가 아직 1인 상태로 메모리 해제가 이루어 지지 않는 것을 확인 할 수 있다.

이러한 경우 어떻게 해결할 수 있을까?

---

## 강한 참조 vs 약한 참조
참조를 할 때 변수 앞에 키워드 'weak'를 붙이지 않는다면 모든 참조는 강한 참조로 이루어진다.

앞서 설명한 예시 또한 모두 강한 참조로 이루어져있다.
강한 참조는 모두 reference count가 증가하기 때문에 바로 앞선 예시와 같은 문제가 발생할 수 있다.
이를 해결하기 위해 **약한 참조**를 이용할 수 있을 것이다.

약한 참조는 참조가 되어도 reference count가 증가하지 않는다.

![](https://velog.velcdn.com/images/donotinto/post/979b7996-5cb8-4390-ae8c-07cd0b128545/image.png)

위 예시처럼 원래라면 Character reference count가 2까지 증가하여 a = nil을 해도 참조 카운트가 남아 메모리 해제가 안됐어야 했지만,
약한 참조를 통해 reference count가 증가하지 않아서 메모리 해제가 가능하였다.
