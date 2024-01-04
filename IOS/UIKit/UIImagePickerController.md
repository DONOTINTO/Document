# UIImagePickerController
### 목차
1. [개요](#1-개요)
2. [UIImagePickerController란?](#2-uiimagepickercontroller란)
3. [UIImagePickerController의 Source Type](#3-uiimagepickercontroller의-source-type)
4. [UIImagePickerController가 탑재한 기능들](#4-uiimagepickercontroller가-탑재한-기능들)
5. [UIImagePickerController 사용법](#5-uiimagepickercontroller-사용법)
6. [참고 문헌](#6-참고-문헌)
---
> ## 1. 개요
<br/>
카메라, 사진첩에 접근하는 것은 아마 많은 어플들에 필요한 기능일 것이다.  

<br/>

AddressBook을 만들면서 해당 기능이 필요했고, TIL 주제로 선정하였다.  
UIImagePickerController를 통한 사진 촬영, 사진첩 접근 및 해당 데이터를 가져오는 방법까지 작성하였다.

<br/>  
<br/>  

> ## 2. UIImagePickerController란?  

<br/>
<img width="844" alt="UIImagePickerController" src="https://user-images.githubusercontent.com/123792519/225911444-b2a3fef8-5c84-4a00-83c6-10bfd2273081.png">  

<br/> 
사진을 촬영하고 영상을 녹화하며, 사진첩에서 아이템을 선택하는 시스템 인터페이스를 관리하는 View Controller이다.  

<br/>
image picker controller는 유저의 상호작용을 관리하고 상호작용의 결과를 delegate object에 전달한다. 그 역할과 모습은 ❗️source type
에 의해 결정된다.

<br/>
<br/>
<br/>  

> ## 3. UIImagePickerController의 Source Type
<br/>

- `.SourceTypue.camera`  
사진을 촬영하거나 영상 촬영을 제공한다

- `.SourceType.photoLibrary 또는 .savedPhotoAlbum`  
저장된 사진과 영상을 선택할 수 있는 인터페이스를 제공한다

<br/>
<br/>  

> ## 4. UIImagePickerController가 탑재한 기능들
<br/>
UIImagePickerController가 담고 있는 기본적인 기능들을 사용하고자 한다면 아래 단계를 수행해보면 된다.

---
1. `isSourceTypeAvailable(_:)`메소드를 호출하여 device가 source type의 기능(.camera / .photoLibrary / .savedPhotoAlbum )을 사용할 수 있는지 체크할 수 있다.

```swift
if UIImagePickerController.isSourceTypeAvailable(.camera) {
    // 해당 source type을 사용할 수 있다면 해당 공간의 코드가 실행
    // if 문은 예시일뿐이며 실행 가능 여부를 판단할 수 있는 Bool값을 return하기 때문에 해당 결과값을 따로 이용할 수도 있을 것이다.
}
```
<br/>

2. `availableMediaType(for:)`메소드를 호출하면 해당 device에서 사용 가능한 media type을 얻을 수 있다.

```swift
let mediaType = UIImagePickerController.availableMediaTypes(for: .camera)

print(mediaType)
// >> optional["public.image", "public.movie"]
```
<br/>

3. `.mediaTypes`를 통해 내가 원하는 미디어 타입을 문자열 배열로 설정할 수 있다.  
미디어 타입은 다시 2가지로 앞서 나온 `public.imaged`와 `public.movie`가 존재한다.
```swift
let imagePicker = UIImagePickerController()
imagePicker.mediaTypes = ["public.image", "public.movie"]
// >> 사진과 영상 촬영 모두를 사용한다는 의미이다.
```

4. `present`를 통해 UI를 호출한다.

<br/>  
<br/>  

> ## 5. UIImagePickerController 사용법
<br/>

- UIImagePickerController는 UINavigationController를 상속받고 있기 때문에 `UIImagePickerControllerDelegate`와 `UINavigationControllerDelegate`를 extention에 추가

<br/>

- 기초적인 사용 방법은 다음 코드를 참고하여 활용
```swift
if UIImagePickerController.isSourceTypeAvailable(.camera) {
    let imagePicker = UIImagePickerController()
    if let mediaType = UIImagePickerController.availableMediaTypes(for: .camera) {
        imagePicker.mediaTypes = mediaType
    }
    imagePicker.delegate = self
    imagePicker.sourceType = .camera
    imagePicker.allowsEditing = true
    self.present(imagePicker, animated: true, completion: nil)
}
```
<br/>

- info.plist에 privacy key들을 추가  

    - `Privacy - Microphone Usage Description`  
    음성 녹음 (동영상 촬영에 필요)
    - `Privacy - Photo Library Additions Usage Description`  
    사진첩 추가 (사진첩 접근에 필요)
    - `Privacy - Camera Usage Description`  
    카메라 사용 (카메라 접근에 필요)

<br/>

- 촬영 후 이미지 동작 메소드

```swift
func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {  
    // 이미지 선택이 완료 후 동작
    // info는 [UIImagePickerController.InfoKey]를 담고 있기 때문에 info[.xxxx] 안에 다양한 정보들을 담고 있다.
    // ex)
    let photo = info[.editedImage] as? UIImage
}
```

<br/>
<br/>  

> ## 6. 참고 문헌
[Apple 공식 개발자 문서](https://developer.apple.com/documentation/technologies)
