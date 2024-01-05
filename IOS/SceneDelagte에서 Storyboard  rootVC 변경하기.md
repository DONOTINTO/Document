## 문제 상황
스토리보드로 생성한 VC를 변경해주려고 했다.  
이상하게 스토리보드에서 생성한 VC는 rootVC 변경이 안돼서 스토리보드의 한계라고 생각했다...

## 문제 원인
우선 기존에 작성한 코드를 살펴보며 아래와 같다.
```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
    guard let windowScene = (scene as? UIWindowScene) else { return }
        
    let window = UIWindow(windowScene: windowScene)
    let storyboard = window.rootViewController?.storyboard
    let isLogin = UserDefaults.standard.bool(forKey: Constant.KEY.AUTOLOGIN)

    guard let entryVC = storyboard.instantiateViewController(withIdentifier: Constant.IDENTIFIER.EntryVC) as? EntryViewController else { return }
    guard let mainVC = storyboard.instantiateViewController(withIdentifier: Constant.IDENTIFIER.mainVC) as? ViewController else { return }
        
    window.rootViewController = isLogin ? mainVC : entryVC
        
    self.window = window
    self.window?.makeKeyAndVisible()
}
```
스토리보드가 어딘가 있을거란 안일한(?) 생각을 했던게 엿보이는 코드다...   
사실 스토리보드가 어느 시점에 생성되는지는 잘 모르겠다  ~~진짜 못찾겠다..~~   

다만 저 시점에서는 적어도 내가 찾으려는 Storyboard는 존재하지 않는다.

## 문제 해결
Storyboard가 저 시점에는 없기 때문에 직접 Storyboard 인스턴스를 생성하면 된다.

```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
    guard let windowScene = (scene as? UIWindowScene) else { return }
        
    let window = UIWindow(windowScene: windowScene)
    let storyboard = UIStoryboard(name: "Main", bundle: nil)
    let isLogin = UserDefaults.standard.bool(forKey: Constant.KEY.AUTOLOGIN)
    guard let entryVC = storyboard.instantiateViewController(withIdentifier: Constant.IDENTIFIER.EntryVC) as? EntryViewController else { return }
    guard let mainVC = storyboard.instantiateViewController(withIdentifier: Constant.IDENTIFIER.mainVC) as? ViewController else { return }
        
    window.rootViewController = isLogin ? mainVC : entryVC
        
    self.window = window
    self.window?.makeKeyAndVisible()
}
```