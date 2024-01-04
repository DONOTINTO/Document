# Entry Point란?
Entry Point(진입점)이란, 프로그램이 시작하는 지점을 의미한다.
많은 프로그래밍 언어들은 main()함수를 Entry Point로 사용한다. 마찬가지로 Swift도 main()함수가 존재하지만 @main 이라는 attribute symbol을 사용하여 Swift의 진입점을 알려준다.

![](https://velog.velcdn.com/images/donotinto/post/8e977a48-d57e-4c69-a835-b0f3ef892800/image.png)

## StoryBoard에서의 Entry Point
위 코드와 같이 StoryBoard에서도 마찬가지로 Entry Point를 지정해주어야 Root View Controller가 설정되어 화면이 나타나는듯하다.

![](https://velog.velcdn.com/images/donotinto/post/9d8a9b77-ea58-4b61-92c7-128e0c8730b7/image.png)

story board에서는 위와 같이 화살표 표시로 나타내진다.
위 경우 Tab Bar Controller에 연결된 첫번째 하위 VC(Home VC)가 화면에 그려질 것이다.

## StoryBoard에서 Entry Point가 없다면?

![](https://velog.velcdn.com/images/donotinto/post/2812e38f-3acb-406b-b170-af3c158f9210/image.png)

StoryBoard에서 Entry Point가 없다면 위와 같이 Entry Point를 설정하지 않아 Default VC를 인스턴스화하지 못하였다는 에러 문구와 함께 아무런 화면을 띄우지 못하게 된다.

## StoryBoard에서 Entry Point 설정
기본적으로 Entry Point를 Drag & Drop방식을 통해 원하는 진입 포인트에 두면 된다. 그러나 StoryBoard를 다루다보면 Entry Point가 사라질 수 있는데, VC의 Inspector에서 손쉽게 다시 생성할 수 있다.

![](https://velog.velcdn.com/images/donotinto/post/bc96a655-e950-4155-a286-4b443e6c5488/image.png)

'Is Initial View Controller'를 클릭하면 해당 VC가 Entry Point로 설정되어 사라졌던 Entry Point가 다시 생성된다.

---
[참고 블로그](https://co-dong.tistory.com/56)