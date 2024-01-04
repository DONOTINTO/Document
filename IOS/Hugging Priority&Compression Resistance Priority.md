## 문제상황

![](https://velog.velcdn.com/images/donotinto/post/7eb93a33-fda9-41d5-ad8c-fb0a10799384/image.png)

네이버 웹툰 메인 페이지 UI를 따라만들어 보던 중에, **작가명**과 **작품 평점**을 스택으로 묶어서 관리하였더니 위 사진처럼 앞에 작품 평점이 ...으로 간략화되거나 작가명이 ...으로 간략화되는 문제가 발생했다.

해당 스택의 distribution(분포)를 Fill로 설정하였고 너비가 이미지 너비만큼으로 고정되어있다보니 랜덤으로 우선순위가 적용되는 듯 하였다.

이에 해당 스택에 포함된 **레이블**들의 우선순위를 정해줘야 했다. 내가 원하는 우선 순위는 **작품 평점 레이블** >> **작가명 레이블**로 작품 평점을 무조건 보이도록 하려고 했다.

---

## 문제해결

size inspector에는 이를 해결할 수 있는 수단이 존재하는데

![](https://velog.velcdn.com/images/donotinto/post/bc760c2d-c543-47c2-85eb-ac6822e998bb/image.png)

바로 **Content Hugging Priority**와 **Content Compresstion Resistance Priority**이다.
우선 이 두가지에 대하여 알아보자...

![](https://velog.velcdn.com/images/donotinto/post/3bc8ed3e-4a91-40cc-9297-fb4aa597b48b/image.png)

---

### Content Hugging Priority

Content Hugging Priority는 본래 크기에서 더 커지는 것을 거부하는 우선순위이다.

![](https://velog.velcdn.com/images/donotinto/post/49c73376-575e-47d3-b0b5-57dba8d2c595/image.png)

너비가 고정인 Stack 내부에 2개의 Label이 존재한다.
두 Label은 Stack의 Distribution은 Fill로 설정되어있다.

각 Label의 너비는 작성된 '가나다'의 너비만큼의 본래 크기를 가질것이지만 Stack(Fill)로 묶여 두 레이블 중 하나는 나머지 빈공간을 채워야한다.

이때 Hugging Priority의 우선순위가 높은 쪽이 커지는 것을 거부하기 때문에 우선순위가 낮은 쪽이 빈 공간을 채우게 된다.

---

### Content Compresstion Resistance Priority

Content Compresstion Resistance Priority는 본래 크기에서 더 작아지는 것을 거부하는 우선순위이다.

![](https://velog.velcdn.com/images/donotinto/post/21ea83e2-4fb0-4f09-b232-04d3030d2f6e/image.png)

마찬가지로 너비가 고정인 Stack 내부에 2개의 Label이 존재한다.
두 Label은 Stack의 Distribution은 Fill로 설정되어있다.

각 Label의 너비는 작성된 '가나다라마바사아자차카타파하12345678910'의 너비만큼의 본래 크기를 가질것이지만 Stack(Fill)로 묶여 두 레이블 중 하나는 줄어들어 공간을 만들어줘야 할 것이다.

이때 Compresstion Resistance Priority가 높은 쪽이 작아지는 것을 거부하기 때문에 본래 크기를 유지하게 된다.


### 결과

![](https://velog.velcdn.com/images/donotinto/post/8fbb997b-3668-44f3-866b-383b6f7e0bdf/image.png)

위 문제에서는 **작품 평점 레이블**의 Content Compresstion Resistance Priority를 높여주어 본래 크기를 유지하게 하였다.

---
### 참고
[참고 블로그](https://ontheswift.tistory.com/21)
