#TableViewCell 높이 동적할당

TableViewCell의 높이를 내부 요소들의 사이즈에 맞추어 동적으로 높이가 결정되게 할 수 있다.

## 설정 방법
설정 단계는 크게 세가지로 나뉘어진다.

1. 오토레이아웃 설정
2. estimatedRowHeight 설정
3. automaticDimension 설정

### 1. 오토레이아웃 설정
Cell 내부의 요소들의 높이를 바탕으로 동적으로 할당하기 때문에, Cell 내부의 오토레이아웃이 특히 상하로 제대로 설정되어있어야 한다.

<img width="357" alt="스크린샷 2024-01-08 오후 9 27 45" src="https://github.com/DONOTINTO/Document/assets/123792519/e8de0b44-9e8b-44bc-b7e6-3a57101017c9">

모든 요소들이 상하로 제약이 걸려있는 걸 확인할 수 있다.

### 2. estimatedRowHeight 설정
estimatedRowHeight는 예측되는 높이를 설정해주는 것이다.   
공식문서 설명에 따르면 이렇게 예측되는 높이를 설정해준다면, 동적인 높이를 설정하는데 있어서 테이블 뷰 로딩 속도를 향상시킬 수 있다고 한다.

기본값은 Dimension이고 동적 할당을 사용하려면 값이 0이 되어선 안된다.

```swift
var myTableView = UITableView()

myTableView.estimatedRowHeight = UITableView.automaticDimension
```

### 3. automaticDimension 설정
automaticDimesion은 공식문서를 봐도 무슨 말인지 모르겠으나, 자동으로 계산된 높이 상수인 듯하다.

따라서 Cell의 rowHeight를 automaticDimension으로 설정해주기만 하면 된다.

```swift
var myTableView = UITableView()

myTableView.estimatedRowHeight = UITableView.automaticDimension
myTableView.rowHeight = UITableView.automaticDimension
```

> 특정 셀 고정 / 나머지 동적 할당
```swift
extension ViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        if indexPath.row == 0 {
            return 500
        }

        return UITableView.automaticDimension
    }
}
```


## 참고
[공식 문서 - estimatedrowheight](https://developer.apple.com/documentation/uikit/uitableview/1614925-estimatedrowheight)   
[공식 문서 - automaticdimension](https://developer.apple.com/documentation/uikit/uitableview/1614961-automaticdimension)   
[블로그](https://minzombie.github.io/ios/selfSizing/)