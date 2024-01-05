### UITextField Placeholder

UITextField에는 아무것도 입력하지 않았을 때 나타나는 다음 이미지와 같은 짧은 도움말을 설정할 수 있다.

![](https://velog.velcdn.com/images/donotinto/post/7e132217-53b1-4af6-bc32-ba1614d61062/image.png)

### 문제 상황

사용자에게 도움을 주는 편리한 기능이지만 입력되는 text와는 다르게 placeholder 자체의 text color를 변경하는 직접적인 기능을 제공해주지 않는다.

그래도 다행히 이를 변경할 수 있는 방법은 존재한다.

![](https://velog.velcdn.com/images/donotinto/post/d4b8c65f-fb09-4a19-85e2-8c74f5b101da/image.png)

NSAttributedString 타입인 attributedPlaceholder은 기본값 자체가 nil이지만, 세팅을 통해 내가 원하는 속성들을 부여할 수 있다고 한다. 더불어 다른 textField의 스타일 속성에는 영향을 주지 않는다고 한다.

```swift
textField.attributedPlaceholder = NSAttributedString(string: textField.placeholder!, attributes: [.foregroundColor: UIColor.white])
```

위와 같이 나는 placeholder의 text color를 white로 변경할 수 있었다.
사실 NSAttributedString이 뭔지 저 init함수가 어떤 의미인지는 정확하게 알지 못하겠다.

우선 현재로서는 이렇게도 속성을 부여할 수도 있다는 것만 알고, 다음에 공부해봐야겠다.
