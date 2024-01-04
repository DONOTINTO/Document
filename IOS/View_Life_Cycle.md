# View LIfe Cycle
### 목차
1. [개요](#1-개요)
2. [View Life Cycle의 순서](#2-view-life-cycle의-순서)
3. [View Life Cycle method 종류 및 기능](#3-view-life-cycle-method-종류-및-기능)
5. [참고 문헌](#4-참고-문헌)
---
> #### 1. 개요

<br/>

ViewController의 생명주기로 view가 나타나고 사라지는 과정에서 관련된 메소드들이 호출되어진다.  
이를 잘 활용하면 원하는 타이밍에 내가 원하는 동작을 수행할 수 있다.  

<br/>

> #### 2. View Life Cycle의 순서  

<br/>

![ViewControllerLifeCycle](https://user-images.githubusercontent.com/123792519/227135927-848b213b-50e2-452b-9c19-23cb4365a729.jpeg)

<br/>

> #### 3. View Life Cycle method 종류 및 기능

<br/>

```swift
// 컨트롤러가 관리하는 view를 생성할 때 호출
loadView()

// view가 메모리에 올라갈 때 호출
viewDidLoad()

// view 계층에 view가 추가되기 직전 호출
viewWillAppear(_:)

// view 계층에서 view가 추가된 직후 호출
viewDidAppear(_:)

//  view 계층에서 view가 삭제되기 직전 호출
viewWillDisappear(_:)

// view 계층에서 view가 삭제된 직후 호출
viewDidDisappear(_:)
```

<br/>

> #### 4. 참고 문헌
[Apple 공식 개발자 문서](https://developer.apple.com/documentation/technologies)  
[Apple 공식 개발자 문서 - UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)    
[tistory - DongTaTo](https://co-dong.tistory.com/62)