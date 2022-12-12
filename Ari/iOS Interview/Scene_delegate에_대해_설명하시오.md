# Scene delegate에 대해 설명하시오.

* iOS 12 까지는 하나의 앱에 하나의 윈도우, 즉 한 앱을 동시에 키는 것이 불가능 했었습니다.
* 하지만 iOS 13부터는 window의 개념이 Scene으로 대체되고 하나의 앱에 여러개의 Scene을 가질수 있게 되었습니다.
* 즉 하나의 앱을 동시에 켜는 것이 가능해졌는데, 이 때 SceneDelegate 라는 개념이 생겼습니다.
* 이때부터 AppDelegate의 역할중 UI의 상태를 알 수 있는 UI Life Cycle에 대한 부분을 SceneDelegate가 대체하게 됩니다.


![Session Lifecycle](https://user-images.githubusercontent.com/75905803/206990870-d0a7bc08-5816-430c-b66c-baf73c87f58a.png)

![App Delegate Responsibilities](https://user-images.githubusercontent.com/75905803/206990888-f3659325-b878-4841-9f10-c6d72be1c949.png)

![UISceneDelegate](https://user-images.githubusercontent.com/75905803/206990903-d501beed-da7d-46d4-a17f-9a51bd1896b2.png)

![CAMP](https://user-images.githubusercontent.com/75905803/206990936-72278328-2f44-4ea3-91fb-7258157eb4d8.png)


</br>
</br>

#

<img width="415" alt="Launch Screen UI" src="https://user-images.githubusercontent.com/75905803/206990994-3d3a976f-6a73-418e-b826-7bf05fdd5ae5.png">

위 그림은 애플 문서에서 제공하는 Scene의 상태 전환을 보여주는 그림이다.

* 사용자나 시스템이 앱의 Scene을 요청하면 UIKit이 이를 생성하고 `Unattached`상태로 둔다.
* 요청자가 사용자라면 Scene을 바로 `Foregroun`d로 전환한다.
* 요청자가 시스템이라면 Scene을 `Background`로 전환하여 이벤트를 처리할 수 있도록 한다.
* 사용자가 앱의 UI를 닫으면 UIKit은 Scene을 Background로 전환하고 곧 `Suspended` 상태로 전환한다.
* UIKit은 Background, Suspended 상태의 앱은 언제든지 연결을 끊고 `Unattached` 상태로 전환할 수 있다.

</br>
</br>

## 앱이 실행하는 관점

1. 먼저 UIKit이 Scene을 앱에 연결한다. 그런 뒤 Scene의 초기 UI를 구성하고 필요한 데이터를 로드한다
2. Foreground Active 상태로 전환할 때 UI를 구성하고 사용자와 상호작용할 준비를 한다.
3. Foreground Active > Foreground Inactive로 전환할 때는 데이터를 저장한다
4. Foreground에서 Background로 전환할 땐 중요한 작업을 완료하고 메모리를 확보한 뒤 현재 앱 UI를 스냅샷으로 준비한다. 이 화면이 앱 전환기에서 보이는 그 화면이다.
5. Scene과의 연결이 끊어지면 Scene과 관련된 모든 리소스를 정리한다. (Suspeded 상태)

</br>
</br>

# Reference

- https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle
- https://developer.apple.com/videos/play/wwdc2019/258/
