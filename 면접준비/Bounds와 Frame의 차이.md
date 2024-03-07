# Bounds와 Frame의 차이

View의 위치나 크기를 확인하는 방법은 Bounds와 Frame 프로퍼티를 사용하는 것이다.
둘 다 View 위치나 크기를 확인할 수 있는데 왜 구분해두었는지 알아보자.

## 차이점
두 프로퍼티의 차이점은 위치값이 상대적인지 절대적인지의 차이이다.

예를 들어 아래 뷰가 있다고 가정해보자.

![Group 19](https://github.com/DONOTINTO/Document/assets/123792519/03944f2d-ef6d-4cf2-8b7f-a61a314fa55d)

## Frame

Frame은 superView에 대한 상대적인 위치(좌표값)을 가지게 된다.

![Group 20](https://github.com/DONOTINTO/Document/assets/123792519/09d3ead2-5174-4cb8-b493-30f6ab5d4ca0)

superView의 왼쪽 모서리를 (0,0)으로 기준으로 두었을 때, Frame은 (0,0)으로 부터 얼마만큼 떨어졌는지에 대한 좌표값을 가지게 된다.

View의 Frame.origin은 (55,68)을 가지게 된다.

## Bounds

Bounds는 Frame과 다르게 절대적인 자신만의 좌표값을 지니게 된다.
즉 본인의 위치가 곧 (0,0)이 되는 것이다.

![Group 21](https://github.com/DONOTINTO/Document/assets/123792519/4e5019d5-5ab7-418e-ae2c-fd2970ff945a)

## 참고
[블로그](https://zeddios.tistory.com/203)