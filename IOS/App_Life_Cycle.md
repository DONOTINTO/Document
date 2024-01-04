# APP LIfe Cycle
### 목차
1. [개요](#1-개요)
2. [App Life Cycle이란?](#2-app-life-cycle이란)
3. [App Life Cycle State 종류](#3-app-life-cycle-state-종류)
4. [Application Delegate method & Scene Delegate method](#4-application-delegate-method--scene-delegate-method)
5. [참고 문헌](#5-참고-문헌)
---
> #### 1. 개요

<br/>

앱이 실행되고 종료되기 전까지 많은 상태가 존재한다고 한다.  
그 상태는 무엇이 있고, 그 역할에 대해 알아보려고 한다.  
애플 공식 개발 문서를 토대로 내가 이해한 내용을 정리하였다.

<br/>

> #### 2. App Life Cycle이란?  

<br/>

기본적으로 IOS에서 App의 현재 상태에 따라 무엇을 할 수 있고 할 수 없는지 결정된다.

UIKit은 적절한 delegate 객체의 메소드를 호출하여 앱의 상태가 변한 것을 알려준다. 이렇듯 각 상태 별 적절한 조치를 취할 수 있도록 만들어진 것이 App Life Cycle 개념이다.

<br/>

**IOS 12** 이 전 버전에서는 UIApplicationDelegate 객체를 통해 Life Cycle 이벤트에 대응한다.  

**IOS 13** 이 후 버전에서는 다중 창(multi-window)과 멀티캐스팅에 대응하기 위하여 scene을 기반으로 UIScenceDelegate 객체를 통해 Life Cycle 이벤트에 대응한다.  

App Delegate에서 담당하던 모든 처리를 Scene Delegate로 분리되었을 뿐 App Life Cycle 자체는 동일하다.

<br/>

> #### 3. App Life Cycle State 종류

<br/>

<img width="509" alt="스크린샷 2023-03-23 오후 3 35 42" src="https://user-images.githubusercontent.com/123792519/227122944-d9482981-2303-4cb1-84ca-fe1b8fc19a74.png">


- **unattached**  
scence이 앱에 아직 연결되지 않은 상태

- **foreground Inactive**
app이 실행중이지만, scene이 아무런 event를 받을 수 없는 상태

- **foreground Active**
app이 실해중이고, scene에서 event를 받고 있는 상태

- **background**
app이 background에서는 실행 중이지만, 화면 상에 나타나지는 않은 상태

- **suspended**
app의 관련 데이터를 메모리에만 올려둔 상태로, background에 진입하여 아무런 동작을 하지 않는다면 suspended 상태가 된다.  
메모리가 부족하면 suspended 상태의 앱은 메모리에서 가장 먼저 해제된다.

<br/>

> #### 4. Application Delegate method & Scene Delegate method

<br/>

```swift    
// 1. 앱 실행 후 모든 설정이 완료된 뒤 호출
application(_:didFinishLaunchingWithOptions:)

// 2. 앱 delegate에 scene이 추가되었음을 알려줌
scene(_:willConnectTo:options:)

// 3. 앱이 background에서 foreground로 변환 될 때 호출   
// (background -> foreground.inactive)
sceneWillEnterForeground(_:)

// 4. 앱이 inactive에서 active 상태로 변경될 때 호출  
// (foreground.inactive -> foreground.active)
sceneDidBecomeActive(_:)

// 5. 앱이 active에서 inactive 상태로 변경될 때 호출
// (foreground.active -> foreground.inactive)
sceneWillResignActive(_:)

// 6. 앱이 background로 진입했을 때 호출
sceneDidEnterBackground(_:)

// 7. 앱이 시스템에 의해 종료될 때, UIKit에 연결된 Scene을 해제해달라고 요청 할 때 호출
sceneDidDisconnect(_:)
```

<br/>

> #### 5. 참고 문헌
[Apple 공식 개발자 문서](https://developer.apple.com/documentation/technologies)  
[Apple 공식 개발자 문서 - Managing your app’s life cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle#life-cycle)  
[Apple 공식 개발자 문서 - UIScene.ActivationState](https://developer.apple.com/documentation/uikit/uiscene/activationstate)  
[velog - rnfxl92](https://velog.io/@rnfxl92/앱-생명주기-Application-Life-Cycle)  
[tistory - 유정주](https://jeong9216.tistory.com/461)