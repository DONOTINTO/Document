# UIScrollView
### 목차
1. [개요](#1-개요)
2. [UIScrollView란?](#2-uiscrollview란)
3. [UIScrollView의 구조](#3-uiscrollview의-구조)
4. [UIScrollView In Storyboard](#4-uiscrollview-in-storyboard)
5. [참고 문헌](#5-참고-문헌)
---
> #### 1. 개요
<br/>
UIScrollView는 편리하지만 초기 설정을 이해하기 어려운 부분들이 많았다.  

초기 설정(제약)부분과 구조 위주로 내용을 정리하였다.

<br/>

> #### 2. UIScrollView란?  

<br/>
<img width="844" src="https://user-images.githubusercontent.com/123792519/225593092-aff4b60b-bdcd-4665-a327-bc0295fd2ffa.png">  

**기능적**으로는 스크롤 뷰에 포함된 뷰들의 스크롤 및 줌을 허용하며,  

**구조적**으로는 UIScrollView는 UITableView, UITextView를 포함한 몇몇의 UIKit의 super class라고 소개한다.  
<br/>  

> #### 3. UIScrollView의 구조

<br/>

UIScrollView는 content view에서 조정이 가능한 원점을 가진 뷰이며, frame에 맞게 content를 clip한다.  

<br/>
이는 StoryBoard를 보면 더 정확히 이해할 수 있다.  

<br/>  

StoryBoard에 ScrollView 객체를 추가하면 하위에 `Content Layout Guide`과 `Frame Layout Guide`라는 component가 함께 생성된다.  

- Content Layout guide  
Content가 담기는 Layout으로 Scroll View 안에 들어가는 모든 Content를 담고 있는 영역이다.  

- Frame Layout  
Scroll View 안에서 Frame 크기에 맞게 clip되어 보여지는 영역이다.  

- Content View  
실질적으로 보여지는 view이다.

<br/>

> #### 4. UIScrollView In Storyboard
<br/>

**❗️세로 스크롤을 예시로 설명하고 있습니다❗️**  

<br/>

`1. Scroll view는 Safe Area 4 방향에 contraint를 설정한다.`  

-  스크롤 뷰 특성 상 **좌우 스크롤**, **상하 스크롤** 인지 모르기 때문에 width와 height가 모호하다는 오류가 발생한다. 우선 무시하자.

`2. Content View는 Centent Layout Guide 4 방향에 contraint를 설정한다.`  

- 이를 통해 Centent Layout Guide가 Content View의 크기 (height)에 맞춘다고 생각하면 편하다.  
하지만 아직 height를 명확하게 설정해준 것은 아니기 때문에 오류가 사라지지는 않는다.

`3. Content View의 width를 Frame Layout Guide와 일치시킨다.`  

- 이 또한 스크롤 뷰 특성 상 **좌우 스크롤**, **상하 스크롤** 인지 모르기 때문에 화면 크기에 맞추어 보여주는 Frame Layout Guide의 width와 일치시키주면 width가 모호하다는 오류는 사라진다.

`4. Content View 안에 넣고 싶은 component들을 추가한다.`

`5. Content View의 Height를 지정한다.`

두가지 방법이 존재한다.  
   1. Content View의 크기를 원하는 크기만큼 지정한다.  
   Scroll View가 스크롤 할 수 있는 영역의 크기는 결국 Content View의 크기에 결정된다. 그렇기 때문에 Content View 크기를 직접적으로 지정하는 방법이 있다.  

   2. Content View 내부 최상단 component는 ContentView의 Top과 제약조건을 걸고, 최하단 componentsms ContentView의 Bottom과 제약조건을 건다. 


> #### 5. 참고 문헌
[Apple 공식 개발자 문서](https://developer.apple.com/documentation/technologies)
