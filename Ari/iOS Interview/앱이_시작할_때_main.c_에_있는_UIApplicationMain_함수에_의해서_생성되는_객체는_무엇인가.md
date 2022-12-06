# 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?

![image](https://user-images.githubusercontent.com/75905803/205817272-652d42d8-4e1a-443a-9b31-8448094126de.png)


* UIApplication 객체가 생성됩니다. 또한 AppDelegate, SceneDelegate 객체 또한 생성됩니다.
* 마지막으로 앱의 루트가 되는 Window 객체를 생성하고 최초 뷰 컨트롤러를 할당하게 됩니다.

#

## UIApplication은 어떤 역할을 하나요?
* UIApplicationMain에서 만들어지는 싱글톤 객체입니다.
* 최초의 런루프를 만들고 AppDelegate에게 Delegate를 위임합니다.
* 시스템에서 발생하는 이벤트가 UiKit에 의해 UIEvent로 만들어지면 해당 이벤트를 이벤트큐에 전달합니다.
