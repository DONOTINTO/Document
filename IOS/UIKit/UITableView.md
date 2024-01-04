# UITableView
### 목차
1. [개요](#1-개요)
2. [UITableViewDelegate](#2-uitableviewdelegate)
3. [UITableViewDataSource](#3-uitableviewdatasource)
4. [자주 사용하는 Method](#4-자주-사용하는-method)
5. [UITableViewCell](#5-uitableviewcell)
6. [IndexPath](#6-indexpath)
7. [참고 문헌](#7-참고-문헌)
---
> #### 1. 개요
<br/>
UITableView는 정해진 틀(Cell) 안에서 내가 원하는 데이터를 효과적으로 보여주는 가장 기본적인 View라고 생각한다.  
가장 기본적이고 자주 사용하는 View인 만큼 그 내용을 정리하였다.

<br/>

> #### 2. UITableViewDelegate
UITableView의 protocol  

아래 특징들을 가진 메소드들을 관리한다.  

---
- 커스텀 헤더와 풋터의 생성과 관리
- 헤더와 풋터, rows(행/셀)의 높이 조절
- 행 선택에 대한 응답
- 테이블 행의 스위프와 action에 대한 응답
- 테이블 컨텐츠 수정  
---  
<br/>  

> #### 3. UITableViewDataSource
UITableView의 protocol  
<br/>
테이블 뷰는 데이터를 화면에 표시만 할 뿐이고, 데이터를 직접적으로 관리하지는 않는다.  
**데이터 소스 객체**(해당 protocol을 구현하는 객체)는 테이블의 데이터 관련 요청에 응답한다.  
그 외 아래 특징들을 책임지고 있다. 

  ---
  - 표의 섹션과 행의 갯수를 파악
  - 테이블 각 행에 테이블 뷰 셀을 제공
  - 테이블 섹션 별 헤더와 풋터를 제공
  - 테이블의 인덱스를 설정
  - 기본 데이터의 변경이 필요한 유저 또는 초기화된 테이블의 업데이트에 응답
---
아래 두 함수는 UITableViewDataSource 프로토콜을 추가하면 반드시 정의해야 하는 메소드이다.
<br/>

```swift
// Return the number of rows for the table.
// 테이블 행의 갯수를 반환    
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
   return 0
}

// Provide a cell object for each row.
// 각 행에 셀 객체를 제공
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
   // Fetch a cell of the appropriate type.
   let cell = tableView.dequeueReusableCell(withIdentifier: "cellTypeIdentifier", for: indexPath)
   
   // Configure the cell’s contents.
   cell.textLabel!.text = "Cell text"
       
   return cell
}
```  
<br/>  

> #### 4. 자주 사용하는 Method
```swift
// 각 행에 셀 객체를 제공
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell

// 테이블 행의 갯수 설정
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int

// 해당 행이 선택됨
func tableView(UITableView, didSelectRowAt: IndexPath)

// 해당 행의 editing 스타일을 반환 (.delete / .insert / .none)
func tableView(UITableView, editingStyleForRowAt: IndexPath) -> UITableViewCell.EditingStyle

// 해당 행에 위치한 셀의 insert 또는 delete를 commit하도록 요청
func tableView(UITableView, commit: UITableViewCell.EditingStyle, forRowAt: IndexPath)
/* 함께 알아두어야함
삭제 순서
-> 셀 데이터 삭제 -> 행 삭제
행을 삭제할 때
tableView.deleteRows(at: [IndexPath], with: UITableView.RowAnimation)

행을 삽입할 때
tableView.insertRows(at: [IndexPath], with: UITableView.RowAnimation)
*/
```
<br/>  

> #### 5. UITableViewCell
cell을 생성하는 방법만 알아보자
```swift
// cellForRowAt 메소드에서 셀을 만들어서 반환해주면 해당 셀이 테이블 뷰에 적용된다.
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

    // dequeue구조 형태의 재사용 가능한 셀로 tableView에서 dequeueReusableCell 메소드로 생성이 가능하다.
    // 다만 커스텀 셀을 사용하는 경우 다운 캐스팅이 필요하다
   let cell = tableView.dequeueReusableCell(withIdentifier: "cellTypeIdentifier", for: indexPath)
       
   return cell
}
```
<br/>  

> #### 6. IndexPath
table View method에서 자주 볼 수 있는 IndexPath는 인덱스의 상대적인 경로를 말한다.

테이블 뷰는 기본적으로 섹션과 행으로 나누어진다.  

```swift
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
   // Fetch a cell of the appropriate type.
   let cell = tableView.dequeueReusableCell(withIdentifier: "cellTypeIdentifier", for: indexPath)
   
   // Configure the cell’s contents.
   cell.textLabel!.text = "Cell text"
       
   return cell
}
```
위 method에 indexPath의 파라미터 이름을 보면 cellForRowAt으로  
현재 해당 method가 실행되고 있는 cell이 어느 행에 위치한지를 indexPath라는 상대적인 경로를 통해 알려주고 있다고 볼 수 있다.

**요약**

indexPath -> [section, row] 형태  
- indexPath.section을 통해 해당 cell이 몇 번 째 섹션에 위치하고 있는지 알 수 있다.  
- indexPath.row를 통해 해당 cell이 몇 번 째 행에 위치하고 있는지 알 수 있다.  
<br/>   

> #### 7. 참고 문헌
[Apple 공식 개발자 문서](https://developer.apple.com/documentation/technologies)