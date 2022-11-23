# PCB (Process Control Block)

* 특정한 프로세스를 관리할 필요가 있는 정보를 포함하는 운영체제 커널의 자료구조를 의미한다.
* 따라서 PCB는 운영체제가 프로세스를 표현한 것이라고 할 수 있다.

> PCB 도식화

![image](https://user-images.githubusercontent.com/75905803/203545118-09ca3d31-2f8c-469b-b2f8-23b682b42395.png)

* 구성요소(구조체로 유지)
  * (1) OS가 관리 상 사용하는 정보
    * Process state, Process ID
    * scheduling information, priority
  * (2) CPU 수행 관련 하드웨어 값
    * Program counter, registers
  * (3) 메모리 관련
    * Code, data, stack의 위치 정보
  * (4) 파일 관련
    * Open file descriptors...

</br>

# Context Swiching (문맥 교환)

CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정.

> 예를 들어 A프로세스를 실행할 땐 A프로세스의 메모리 영역을 올리고, B프로세스를 실행할 땐 B프로세스를 메모리 영역을 올리고 이렇게 프로세스를 왔다갔다 하면서 작업해주는 것을 Context Swiching이라고 한다.

![image](https://user-images.githubusercontent.com/75905803/203545653-892ed722-8540-4b51-931f-5d53d2bef50a.png)

CPU가 다른 프로세스에게 넘어갈 때 운영체제는 다음을 수행한다.

* CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
* CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어옴 
* CPU에 실행할 프로세스를 교체하는 기술을 의미한다.

</br>

# Reference

- [반효경 교수님의 운영체제 강의(2014)](https://core.ewha.ac.kr/publicview/C0101020140318134023355997?vmode=f)
