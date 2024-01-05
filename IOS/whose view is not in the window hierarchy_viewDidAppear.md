## 오류 상황   
rootVC에서 UserDefaults를 활용해 로그인 정보를 통해 자동 로그인하여 다음 VC로 넘어가도록 하였더니, whose view is not in the window hierarchy 오류가 발생했다.

~~SceneDelegate에서 아무리 storyboard에 VC를 호출하여도 안되었다. 아무래도 Storyboard로 VC를 만들었기 때문이 아닌가 싶다.~~    

## 오류 원인
해당 오류의 원인은 present를 통해 다음 VC를 호출하는 과정을 ViewDIdLoad에서 실행하였기 때문이다.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // 뷰 호출 코드
}
```

## 오류 해결
이를 해결하기 위해서는 '뷰 호출 코드'를 viewDidLoad가 아닌 viewDidAppear에서 호출해주면 된다.   

이를 View에 Life Cycle과 관련되어있다.   
[ViewLifeCycle](https://github.com/DONOTINTO/Document/blob/main/IOS/View_Life_Cycle.md)에 정리한 내용을 보면 알 수 있듯이 viewDidLoad는 view가 메모리에 올라갈 때 호출이 된다.

즉, VC가 관리하는 Vieww 계층에 아직 view가 추가되지도 않고 메모리만 올라간 상태에서 다음 VC를 호출하니 계층이 꼬이게 된 것이다.

#### 정상적인 호출
> rootVC 생성 - rootVC view 계층 추가 - nextVC 호출 및 생성 - view 계층 추가   

#### 비 정상적인 호출
> rootVC 생성 - ~~rootVC view 계층 추가~~ - nextVC 호출 및 생성 - view 계층 추가

## 한 줄 요약
- 컨트롤러의 View계층이 추가된 이후(**viewDidAppear**) 다음 컨트롤러를 호출할 수 있다.   
