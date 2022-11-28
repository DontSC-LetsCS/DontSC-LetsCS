# Bounds와 Frame의 차이점을 설명하시오.

* Frame은 슈퍼뷰 좌표계에서 뷰의 위치와 크기를 나타냅니다. `뷰의 위치 및 크기를 설정`할 때 주로 사용하게 됩니다.
* Bounds는 자신의 좌표계에서 뷰의 위치와 크기를 나타냅니다. `뷰를 회전한 후 뷰의 실제 크기를 알고싶을 때` 사용합니다.
* 둘의 차이점은 Frame의 사이즈 경우 뷰 영역을 모두 감싸는 사각형으로 나타냅니다. 
* 하지만 Bounds는 뷰 자체의 영역을 나타냅니다. 
  * Frame과 다르게 뷰영역을 모두 감싸서 만든 사각형이 아니라 `뷰 자체의 영역`을 나타냅니다.

#

## Bounds를 사용하는 예시가 어떤게 있나요?
* View 내부에 그림을 그릴 때(drawRect), 혹은 ScrollView에서 스크롤링 할 때 사용합니다.
    * ScrollView의 SubView 중 어떤 영역을 보여줄지 정할 때 사용할 수 있습니다.

</br>

---

</br>

# Frame

- https://developer.apple.com/documentation/uikit/uiview/1622621-frame

Super View 좌표계에서 View의 위치와 크기를 나타낸다.



## Frame의 origin(x, y)

* Super view의 원점을 (0,0)으로 놓고 원점으로부터 얼마나 떨어져 있는지를 나타낸다.
* 따라서 Frame의 origin 값을 변경하면 SubView도 그만큼 같이 움직인다.


## Frame의 Size(width, height)

* View 영역을 모두 감싸는 사각형으로 나타낸다.
* View 자체의 크기가 아니라 View가 차지하는 영역을 감싸서 만든 사각형이라고 이해하면 된다.


## 언제 사용할까?

* UIView의 위치 및 크기를 설정할 때 사용한다.

</br>
</br>

# Bounds

- https://developer.apple.com/documentation/uikit/uiview/1622580-bounds

자신의 좌표계에서 View의 위치와 크기를 나타낸다.

## bounds의 origin(x, y)

* Super view와는 아무 상관 없으면 기준이 자기 자신이다.
* 따라서 좌표의 시작점을 자기의 원점(0,0)으로 놓는다.
* bounds를 바꿔줘야 하는 경우는 ScrollView의 ContentOffset을 설정할 때이다.

## Bounds의 size(width, height)

* View 자체의 영역을 나타낸다.
* Frame과 다르게 View영역을 모두 감싸서 만든 사각형이 아니라 View 자체의 영역을 나타낸다고 이해해보면 된다.

## 언제 사용할까?

* View를 회전(transfomation)한 후 View의 실제 크기를 알고 싶을 때 사용한다.
* View 내부에 그림을 그릴 때(drawRect) 사용한다.
* ScrollView에서 스크롤링 할 때 사용한다.
