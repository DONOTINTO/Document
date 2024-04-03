# scene delegate에 대해 설명하시오.

iOS 13 부터 도입됐으며, 앱당 하나의 window만을 가졌던 과거에 비해 앱당 여러개의 scene을 가질 수 있게되며 생겨났습니다. (아이패드)

scene delegate는 이러한 scene의 생명 주기를 관리하며, 상태 전이에 대한 응답 방법을 정의하고 있습니다.

Scene에는 UI의 인스턴스를 나타내는 windows와 viewcontroller들이 들어있습니다.

scene에 해당하는 UIWindowSceneDelegate를 가지고 있어 UIKit과 앱간 상호작용에 사용됨.

Scene들은 같은 메모리와 앱 프로세스 공간을 공유하면서 동시에 실행됨.
하나의 앱이 여러개의 scene과 scene delegate 객체를 동시에 활성화할 수 있게 도와줌