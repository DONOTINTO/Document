# 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?

기존 Object-c에서는 main함수가 실행되면, UIApplicationMain함수를 호출하고 UIApplication객체를 생성한다.

이 UIApplication객체는 앱에 본체에 해당하게 되고, info.plist 파일로부터 앱 구성에 필요한 정보들을 로드한다.

이 후 이벤트 루프 및 초기설정을 진행한다.

실행 완료 직전에 앱 델리게이트의 application(_:didFinishLaunchingWithOptions:)메소드가 호출된다.

현재는 @main 어노테이션을 통해 기존 main 함수를 대체하고 @main 클래스를 찾아 실행시킨다.

해당 클래스는 진입점이 된다.