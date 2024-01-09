# View의 width, height 정보를 알 수 있는 타이밍

## 문제상황
Storyboard에서 CollectionView를 safe area의 leading, trailing과 동일하게 맞춰주었다.
이럴 경우 CollectionView.frame.width와 기기(UIScreen.main.bounds.width)가 동일해야하는데, 다음과 같은 경우에는 둘의 값이 같지 않은 경우를 확인할 수 있었다.

<img width="564" alt="스크린샷 2024-01-09 오후 10 47 28" src="https://github.com/DONOTINTO/Document/assets/123792519/0b2ac711-346e-4ac8-9ea3-f9794fe6bf0a">

- 시뮬레이터와 스토리보드의 기기가 동일하지 않은 경우

물론 UIScreen.main.bounds.width를 사용하면 되겠지만, CollectionView가 기기와 너비가 다른 경우도 있을 수 있기때문에 CollectionView의 너비를 온전히 가져오는 방법을 찾아보았다.

## 원인
StoryBoard에서 VC를 인스턴스화하여 viewDidLoad가 호출될 때는, 아직 VC가 메모리만 올라가고 View가 완전히 그려지지 않은 상태다. 이 때문에 아직 View(여기서는 CollectionView)의 너비를 알 수 없어, 실제 기기가 아닌 Storyboard에 선택된 기기 정보를 통해 너비를 가져오면서 해당 문제가 발생한다.

실제로 스토리보드가 아닌 코드로 작성 시에는 viewDidLoad에서 너비가 0으로 나온다. 스토리보드처럼 임시로 가져다 쓸 데이터도 없기 때문인 것 같다.

## 해결
VC의 View가 완전히 그려진 이 후인 ViewDidAppear에서 CollectionView.frame.width를 호출하면 실제 기기의 너비와 동일한 값을 가져오는 것을 확인할 수 있다.

<img width="419" alt="스크린샷 2024-01-09 오후 10 12 38" src="https://github.com/DONOTINTO/Document/assets/123792519/fb8f6442-4dbb-433c-8d92-5c6f17f4b6f5">

그래서 collectionView에 대한 세팅은 ViewDidAppear에서 모두 처리해주었다. 다만 View 그려진 뒤 호출되는 함수이기 때문에 구문 마지막에 reloadData를 호출해주었다. (옳은 방식인지는 모르겠다.)

## 참고
[스택오버플로우](https://stackoverflow.com/questions/55667820/frame-size-of-uicollectionview-is-bigger-than-size-of-uiscreen)