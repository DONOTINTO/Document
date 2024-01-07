# UIButton Configuration이란?

IOS 15버전 이후로 UIButton에 적용된 새로운 버튼시스템으로, 버튼의 모양과 동작들을 UIButton Configuration을 통해 구성할 수 있게 되었다.

---
## IOS 15 이후 UIButton에 변경 된 점들
우선 IOS 15 이후 UIButton에 새로 생긴 기능들부터 살펴보면 다음과 같다.

1. Style 추가 (Plain, Gray, Tined, Filled)
2. multiline Text 지원
3. Dynamic Type 지원
4. subtitle 지원
5. activity indicator

---
## UIButton Configuration 적용 방법

UIButton.Configuration은 UIButton의 static func으로 UIButton을 통해 직접 생성이 가능하다.
![Alt text](<스크린샷 2024-01-07 오후 5.41.29-1.png>)

```swift
let button = UIButton()
let btnConfiguration = UIButton.Configuration.plain()
```

configuration을 통해 UIButton의 다양한 것들을 수정할 수 있다.   
(기존 setTitle과 같은 메소드들보다는 우선순위가 낮다.)

```swift
btnConfiguration.title = "타이틀 변경"
btnConfiguration.subtitle = "서브타이틀 변경"
btnConfiguration.image = UIImage(systemName: "pencil")
btnConfiguration.imagePadding = 10
btnConfiguration.background.backgroundColor = .clear

button.Configuration = btnConfiguration
```

---
## 예시 상황
button의 background 컬러를 설정해주어도 위와 같이 state 변경 시 background 컬러가 자기 마음대로 적용된다.

```swift
button.backgroundColor = .clear

button.setImage(UIImage(systemName: "checkmark.square"), for: .normal)

button.setImage(UIImage(systemName: "checkmark.square.fill"), for: .selected)
```

> selected 전
![Alt text](<스크린샷 2024-01-07 오후 5.52.52.png>) 

> selected 후
![Alt text](<스크린샷 2024-01-07 오후 5.53.02.png>)

위 상황에서 configuration의 backgroundColor를 적용해주면 원하던 모양을 얻을 수 있다.
![Alt text](<스크린샷 2024-01-07 오후 5.59.12.png>)

---
## 참고
[참고 블로그](https://gyuios.tistory.com/126)   
[참고 블로그](https://zeddios.tistory.com/1291)   
[공식 문서](https://developer.apple.com/documentation/uikit/uibutton/configuration)   