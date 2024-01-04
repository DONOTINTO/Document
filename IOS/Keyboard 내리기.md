## Keyboard 내리기

어플을 쓰다보면 너무 자연스러운 타이밍에 키보드가 내려간다.
OS에서 알아서 처리해주나 싶었지만 개발자가 모두 처리를 해줘야한다...


### endEditing
VC의 property인 view에는 다양한 기능들이 숨겨져 있다.
그 중 하나로 endEditing은 기존 response가 끝났다는 것을 알려주는 함수이다.
적절한 타이밍에 해당 함수를 호출해준다면 키보드를 자연스럽게 내릴 수 있다.

![](https://velog.velcdn.com/images/donotinto/post/94bfe298-5abd-41c7-b27a-b17d32ed11bd/image.png)

VC의 touchesBegan 함수로 VC에서 터치가 이루어질 때 실행되어 키보드 입력 이 후 앱의 화면을 터치하면 키보드를 내린다.


### Did End On Exit(Story Board - UITextfield)

StoryBoard에서도 아래 기능을 사용하면 키보드를 내릴 수 있다.

![](https://velog.velcdn.com/images/donotinto/post/435f2382-8794-4166-9ff2-9a54d0e11ece/image.png)

![](https://velog.velcdn.com/images/donotinto/post/7556cb9c-6d6d-434d-81e6-ce115269a10d/image.png)

UITextfield의 action을 연결 시 Did End On Exit으로 연결한다면 별다른 코드를 입력하지 않아도 키보드 입력 후 return key를 클릭하면 키보드가 내려간다.

### tap gesture recognizer

UITapGestureRecognizer는 UIGestureRecognizer의 하위 클래스로 탭 제스처를 인식하는 오브젝트(?)이다.
이 탭 제스처를 통해 키보드를 내릴 수 있다.

#### StoryBoard
스토리보드에서는 라이브러리에서 Tap Gesture Recognizer를 View에 추가해주면 된다.

![](https://velog.velcdn.com/images/donotinto/post/10c0d473-94be-4941-ac77-4f865a4ad28b/image.png)

Tap Gesture Recognizer를 추가한 VC를 확인해보면 세번쨰에 파란색으로 추가되었음을 확인할 수 있다.
이를 코드로 연결하여 view에 tap을 인식하여 원하는 반응을 추가할 수 있다.

![](https://velog.velcdn.com/images/donotinto/post/65baa869-9bfc-492b-9c9d-7a094b0230dc/image.png)

위와 같이 탭을 할 때 view.endEditing 함수를 사용하여 키보드를 내려줄 수 있다.

#### Programmatically

코드로는 다음과 같이 설정할 수 있다.
![](https://velog.velcdn.com/images/donotinto/post/e85f02e3-7fff-4d5c-9bcb-57351e3e50c1/image.png)

UITapGestureRecognizer 객체를 생성하여 해당 제스처에 addTarget을 통해 함수를 연결시켜준다.

이 후 내가 탭 저스처를 인식하려는 대상에게 addGestureRecognizer를 통해 앞서 생성한 객체를 추가해준다.

여기서 주의할 점은 인식하려는 대상 객체의 isUserInteractionEnabled를 true로 설정해주어야 한다.

이걸 몰라서 한참 헤매였다..

![](https://velog.velcdn.com/images/donotinto/post/75023b8a-5956-4155-8bf7-2507a59145ad/image.png)

이 함수 내부에 이제 view.endEditing 함수를 호출해주면 키보드 내리기는 끝이다.

UITapGestureRecognizer는 따로 정리해야겠다.

---
[참고-스택오버플로우](https://stackoverflow.com/questions/41772783/tap-gesture-recognizer-added-to-uilabel-not-working)   
[참고-블로그](https://bibi6666667.tistory.com/330)   
[참고-공식문서](https://developer.apple.com/documentation/uikit/uitapgesturerecognizer)