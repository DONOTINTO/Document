# Delegate 패턴이란?

디자인 패턴 중 하나인 Delegate 패턴의 개념은 다음과 같다.
> 하나의 객체가 자신의 책임을 다른 객체에게 위임(Delegate)한다.

## Delegate 패턴를 이해하기 위한 간편 예시

델리게이트 패턴을 이해하기 위하여 다음 가정을 해보겠다.

A - 축구 감독   
B - 축구 선수 (공격수)     
Book - 전략 키워드   

- Book에는 attack 전술이 있는데, A는 특정 타이밍에 attack 전략을 수행하라는 신호를 보낼 수 있다.

- B는 A의 대리자로 설정되어 있어서 A의 신호에 맞추어 요청을 수행할 수 있다.

- 신호를 받은 B가 attack 전략을 수행한다.

```swift
protocol Book {
    func attack(message: String)
}

class DirectorA {
    var delegate: Book?

    //게임 시작 50분이 지났을 때 동작
    func afterFiftyMinutes() {
        delegate?.attack(message: "공격 ㄱㄱ")
    }
}

class PlayerB: Book {
    
    func setDirector() {
        let director = DirectorA()
        director.delegate = self
    }
    
    func attack(message: String) {
        print("수신받은 내용 "\(message)"")
        print("닥공 시작!")
    }
}

let playerB = PlayerB()
playerB.setDirector()
// 공격 ㄱㄱ
// 수신받은 내용 "공격 ㄱㄱ"
// 닥공 시작!
```

## Delegate 패턴 정리
위 예시를 바탕으로 다시 정리하자면 이렇다.

> Book = Delegate Protocol   
> A = Event Listner   
> B = Delegator

1. 이벤트 리스너는 델리게이트 객체를 프로토콜 옵셔널 타입으로 선언한다. (옵셔널 타입이기 때문에 초기화를 진행하지 않으려면 var 변수 형태로 선언해야한다.)

2. 델리게이터는 이벤트 리스너 델리게이트 프로퍼티에 자기 자신을 대입해준다.

3. 리스너 델리게이트 프로퍼티의 타입은 Book이고 델리게이터의 타입은 PlayerB이기에 2번이 동작할 수 없다. 이에 델리게이터는 Book 프로토콜을 채택해준다.

4. Book 프로토콜을 채택한 PlayerB는 Book에서 요구하는 attack 메서드를 준수한다.

5. 이벤트 리스너는 이벤트를 기다리다 이벤트가 실행되면 Delegate에 이를 알린다.

6. 이벤트 리스너의 델리게이터로 설정된 델리케이터는 이벤트 리스너의 수신에 맞추어 준수된 액션을 실행한다.

> **한줄 정리**   
이벤트 리스너의 일을 이벤트 리스너의 요청에 맞추어 델리게이터가 대리로 수행한다.

## 실제 활용 예시
A VC를 통해 B VC를 present한다.   
이때 A의 데이터를 B에게 넘겨주는 것은 어렵지 않을 것이다.
A에서 B인스턴스를 생성하기 때문에 B인스턴스에 그대로 값을 넘겨주기만 하면 되기 때문이다.   
반대의 상황은 어떨까?

B에서 A에게 정보를 전달하거나 특정 액션을 요청하기는 어려울 것이다.   
이때 델리게이트 패턴을 활용하면 B는 A에게 정보를 전달하거나 액션을 요청할 수 있다.   

B는 이벤트 리스너, A를 델리게이터로 설정하여 B에서 Delegator에게 요청을 할 수 있을 것이고, Delegator로 설정된 B에서 요청된 사항을 준수한 프로토콜 액션을 수행할 것이다.


## Delegate Protocol은 AnyObject을 채택해야 한다.

<img width="562" alt="스크린샷 2024-01-11 오후 1 47 40" src="https://github.com/DONOTINTO/Document/assets/123792519/b39918d3-72b3-4358-88a3-50e314d2a990">

<img width="457" alt="스크린샷 2024-01-11 오후 1 47 59" src="https://github.com/DONOTINTO/Document/assets/123792519/9befd812-22f0-41e3-b23f-0985db81e0f1">

DelegatePattern에서 강한 참조를 사용할 경우 위와 같은 상황이 발생할 수 있다.

>Work VC   
    >> WorkVC 생성 : WorkVC ref count + 1   
    >> nextVC 프로퍼티 : Delegate ref count + 1   

> Delegate VC
    >> DelegateVC 생성 : Delegate ref count + 1   
    >> delegate 프로퍼티 : WorkVC ref count + 1   

위 상황이 발생했을 때, Delegate VC가 Dismiss되어도 WorkVC의 nextVC 프로퍼티가 DelegateVC를 참조하고 있기 때문에 메모리에서 해제되지 않는다.   

이 후 WorkVC가 Dismiss되어도 메모리에서 해제되지 않은 DelegateVC의 delegate프로퍼티가 WorkVC를 참조하고 있기 때문에

WorkVC와 DelegateVC는 서로 순환 참조하게 된다.

Delegate패턴에서는 위와같은 상황이 발생할 수 있기 때문에, Delegate를 약한 참조로 선언한다. 

```swift
weak var delegate: DelegateProtocol? //오류 발생
```
다만 Protocol은 구조체, 클래스, 열거형 등 모든 타입에서 채택할 수 있다.   
때문에 위에 delegate에 들어오는 타입이 구조체일지 클래스일지 아무것도 모르는 상태에서는 weak를 사용할 수 없기에 오류가 발생한다.
(weak는 참조 타입에 한하여 사용 가능)

위와 같은 이유로 protocol에는 AnyObject를 채택해주어야 한다.
(AnyObject를 채택하면 클래스 타입만이 해당 프로토콜을 채택할 수 있다.)