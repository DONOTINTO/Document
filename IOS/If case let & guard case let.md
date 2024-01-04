# if case let / gaurd case let 

switch case문을 보다 간결하게 작성하는 방법으로 if case let과 guard case let이 있다. 이는 또 if case / guard case로 줄여서 사용하는 법이 있다.

if case let / guard case let은 주로 연관값이 있는 enum 케이스를 분리하여 작성할 때 사용하는 것 같다.

## `if case / guard case 사용법`
```swift
enum Gender {
    case Male
}

let gildong = Gender.Male
```
위처럼 연관값이 없는 열거체로 만든 'gildong'이 있다고 했을 때, 해당 열거체를 switch문으로 케이스 별로 나눈다면 아래와 같을 것이다. 

> **switch문**
```swift
switch gildong {
    case .Male: 
        print("남자입니다.")
        break
    default: break
}
```
default문과 break 등 추가적으로 작성해주어야하는 부분들이 존재한다.
이를 if case로 변경하면 다음과 같이 작성 할 수 있다.

> **if case 문**
```swift
//gildong이 .Male일때만 통과
if case .Male = gildong {
    print("남자입니다.")
}
```

if case 다음에는 switch와 같이 작성 후 해당 enum으로 생성된 'gildong' 변수를 넣어 케이스를 분리할 수 있다.

보통 대입식에는 변수가 왼쪽으로 오는데 if case는 타입이 먼저와서 어색할 수 있을 것 같다.

> **gaurd case 문**
```swift
//gildong이 .Male일때만 통과
guard case .Male = gildong else {
    return
} 
print("남자입니다.")
```

## `if case 조건이 범위일 때`
그렇다면 if case는 변수가 enum타입일때만 사용이 가능할까?   
그건 아닌 것 같다.

```swift
let temp: Int = .random(in: 1 ... 40)
```
1 부터 100까지 랜덤의 숫자가 담긴 temp라는 변수가 있다고 가정해보자.   
20 단위로 case를 나누어 switch문을 작성해보겠다.

> **switch 문**
```swift
switch temp {
    case 1 ..< 20:
        print("1~19 사이 수 입니다.")
    case 20 ... 40:
        print("20~40 사이 수 입니다.")
    default: break
}
```

이를 if case문으로 수정한다면 다음과 같이 수정할 수 있을 것이다.

> **if case 문**
```swift
if case 1 ..< 20 = temp {
    print("1~19 사이 수 입니다.")
} else if case 20 ... 40 = bmi {
    print("20~40 사이 수 입니다.")
}
```

case를 switch에서 사용했던 방식대로 범위로 지정해주면 된다.

## `if case let / guard case let`
앞서 설명했던 것 처럼 if case let은 enum타입에 연관값이 있을 때 사용이 가능하다.

```swift
enum Gender {
    case Male(age: Int)
    case Female(age: Int)
}

let gildong = Gender.Male(age: 10)
```

이를 switch문으로 사용한다면 연관값을 **바인딩**하여 사용해야 할 것이다.

> **Switch문**
```swift
switch gildong {
    case .Male(age: let temp):   //연관값 바인딩 (바인딩 변수 temp) 
        print(temp)
        break
    case .Female(age: let temp):
        break
}

//output: 10
```

이를 if case let으로 바꾸면 아래와 같다.

> **if case let 문**
```swift
//연관값 바인딩 (바인딩 변수 temp)
if case let .Male(temp) = gildong {   
    print(temp)
} 

//output: 10
```

마찬가지로 guard case let은 다음과 같다.

> **gaurd case let 문**
```swift
//연관값 바인딩 (바인딩 변수 temp)
guard case let .Male(temp) = gildong else {   
    return
}
print(temp)

//output: 10
```