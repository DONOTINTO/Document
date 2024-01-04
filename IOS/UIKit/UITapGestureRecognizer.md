## UITapGestureRecognizer란?

UITapGestureRecognizer란 말 그래도 싱글 탭(터치) 또는 멀티 탭(터치)에 대하여 반응하는 객체이다.

줄여서 탭제스처라고 칭하겠다.

탭제스처는 단독으로 사용되지는 않고, 터치가 되지 않는 객체에 붙어서 터치 이벤트를 만들어주는 용도로 사용한다.(물론 터치가 되는 객체에도 붙일 수 있다.)

가령 Label을 클릭해도 버튼처럼 동작하도록 말이다.

## UITapGestureRecognizer 사용법
탭제스처를 사용하는 방법을 스토리보드 버전과 코드 버전으로 작성하려 한다.

### StoryBoard
![](https://velog.velcdn.com/images/donotinto/post/69674891-0840-44fa-99e7-acda8b5407ae/image.png)

스토리보드의 경우, 탭 이벤트를 원하는 대상에 위와 같이 탭제스처를 추가하면 된다.

![](https://velog.velcdn.com/images/donotinto/post/eb02f7bb-3d3e-4f45-bf88-187024214b7c/image.png)

추가를 하게 되면 위와 같이 스토리보드 레이어에는 탭제스처가 생기고, 컨트롤러 상단 세번째에 저렇게 파란색 원이 생긴다(지금은 클릭하고 캡처를 해서 하얀색으로 보인다...)

스토리보드는 거의 다 끝났다.
이제 해당 VC와 연결된 swift 파일에 액션 이벤트(IBAction)를 추가해주면 된다.

![](https://velog.velcdn.com/images/donotinto/post/ab68d60a-8068-4f62-8b85-e01be54639a5/image.png)

이제 tapGestureRecognizerClicked 이벤트 함수 내부에 내가 원하는 동작을 작성하면 끝이다.

### Programmatically
코드로 작성하는 법은 더 쉽다.

```swift
import UIKit

class testViewController: UIViewController {
    let testLabel = UILabel()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let tapGestureRecognizer = UITapGestureRecognizer()
        tapGestureRecognizer.addTarget(self, action: #selector(testLabelClicked(_:)))
        
        testLabel.isUserInteractionEnabled = true
        testLabel.addGestureRecognizer(tapGestureRecognizer)
    }

    @objc func testLabelClicked(_ sender: UITapGestureRecognizer) {
        
    }
}
```

1. 탭제스처 객체를 생성한다.
2. 탭제스처에 액션 이벤트를 추가한다.(addTarget)
3. 탭 이벤트가 발생할 객체에 탭제스처를 추가한다.(addGestureRecognizer)
4. !!탭 이벤트가 발생할 객체의 isUserInteractionEnabled를 true로 설정한다.
5. 추가한 이벤트의 함수를 작성한다.(예시에서는 testLabelClicked 함수가 될 것이다.)