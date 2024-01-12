# Any와 AnyObject의 차이

Swift에는 Any와 AnyObject라는 타입이 존재한다. 단어에서 알 수 있듯 어떤 타입이든 저장할 수 있을 것만 같다.

## Any
Any는 모든 타입에 대한 인스턴스를 담을 수 있다. 여기서 모든 타입은 클래스, 구조체, 열거형 모든 타입을 포함한다.

```swift
var temp: [Any] = [0, 0.1, "가나다", true, { (a: Int,b: Int) -> Int in return 1}]
```

위와 같이 Int, Float, String, Bool, Closure 등 타입에 상관없이 모든 타입을 설정할 수 있다.

모든 타입을 저장할 수 있기 때문에 컴파일 시에는 어떤 타입을 요소로 가지고 있는지 알 수 없다. 런타임 시 결정되어 해당 타입의 멤버에 접근할 수가 없다.

<img width="416" alt="스크린샷 2024-01-10 오후 9 33 13" src="https://github.com/DONOTINTO/Document/assets/123792519/237f5c1f-2bba-4719-bb70-ca7fb16eb165">

<img width="548" alt="스크린샷 2024-01-10 오후 9 33 37" src="https://github.com/DONOTINTO/Document/assets/123792519/0e308b02-c48f-4e07-91fe-c4c83808336d">

편리한듯 불편한.. 나같으면 불안해서 절대 안쓸거같은 그런 타입이다...

```swift
var temp: [Any] = [0, 0.1, "가나다", true, { (a: Int,b: Int) -> Int in return 1}]

var temp: [Any] = [0, 0.1, "가나다", true, { (a: Int,b: Int) -> Int in return 1 }]

temp.forEach {
    if let a = $0 as? Int {
        print(a)        //0
    } else if let a = $0 as? Double {
        print(a)        //0.1
    } else if let a = $0 as? String {
        print(a)        //가나다
    } else if let a = $0 as? Bool {
        print(a)        //true
    }
}
```
이처럼 오류가 나지 않도록 타입 캐스팅을 진행해주어야 한다.

## AnyObject
AnyObject는 오직 클래스의 인스턴스 즉, 클래스 타입만을 저장할 수 있다.

AnyObject 역시 클래스의 인스턴스이기만 하다면 클래스의 종류와 상관없이 모두 담을 수 있다.

### Protocol AnyObject
protocol을 정의 시 AnyObject를 채택하면 해당 protocol은 클래스에서만 채택할 수 있다.

```swift
protocol ImProtocol: AnyObject { }

class A: ImProtocol { }
struct B: ImProtocol { } //컴파일 에러
```

위 특성 때문에 순환참조가 발생할 수 있는 Delegate 패턴에서는 Delegate Protocol에 AnyObject를 채택하여, delegate를 약한 참조로 설정한다.

[Delegate 패턴](https://github.com/DONOTINTO/Document/blob/main/IOS/디자인패턴/Delegate패턴.md)