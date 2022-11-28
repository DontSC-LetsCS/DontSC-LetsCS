# Dead Lock/Race Condition

- [Deadlock](#deadlock)
- [Race Condition](#reac-condition)

## Deadlock

* 둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 event를 무한히 기다리는 현상 
* 교착상태라고도 하는데, 한정된 자원을 여러 프로세스가 사용하고자 할 때 발생하는 상황을 말한다.
* 예를들어 메인 스레드에서 sync로 작업을 보내는 경우에 Deadlock이 발생한다.
  * 작업을 끝나기를 기다리는 특성 때문이다.
  * main Thread는 sync 작업이 끝나기를, sync는 main Thread의 block-wait이 끝나기를 기다리게 되면서 Deadlock이 발생하게 되는 것이다.
  * 따라서 main Thread에서 sync를 호출하지 않아야 한다.

```swift
let myQueue = DispatchQueue(label: "label")

myQueue.async {
  myQueue.sync {
      // 외부 블록이 완료되기 전에 내부 블록은 시작되지 않습니다.
      // 외부 블록은 내부 블록이 완료되기를 기다립니다.
  }
  // 이 부분은 영원히 실행되지 않습니다.
}
```

* Deadlock은 백그라운드 스레드를 관리하는 큐에서도 발생할 수 있다.
    * serial queue에 sync 블록을 추가해도 deadlock이 발생하게 된다.
    * 이때는 serial queue가 아니라 concurrent 속성으로 바꾸어주어 한번에 여러 작업을 실행할 수 있도록 해주어 해결할 수 있다.

```swift
let myQueue = DispatchQueue(label: "label", attributes: .concurrent)
```

</br>
</br>

## Deadlock 발생 조건 4가지

* 멀티 프로그래밍 환경에서 한정된 자원으로 사용하려고 서로 경쟁하는 상황에 발생한다.
* `상호배제` 사용중인 자원을 다른 프로세스가 사용하려면 요청한 자원이 다 사용되고 난 후 해제될 때까지 기다려야한다.
* `점유대기` 최소한 하나의 자원을 점유하고 있으면서 다른 프로세스에 할당되어 사용하고 있는 자원을 추가로 점유하기 위해 대기하는 프로세스가 있어야 한다.
* `비선점` 다른 프로세스에 할당된 자원은 사용이 끝날 때 까지 강제로 빼앗을 수 없다.
* `순환대기` 대기 프로세스의 집합이 순환 형태로 자원을 대기하고 있어야 한다. 이 조건은 앞서 말한 세 조건을 만족되면 자연스레 만족된다.

</br>
</br>

## Deadlock 처리 방법

- Deadlock Prevention (데드락 예방)
  - 자원 할당 시 데드락의 4가지 발생 조건 중 어느 하나가 만족되지 않도록 하는 것
- Deadlock Avoidance (데드락 회피)
  - 자원 요청에 대한 부가적인 정보를 이용해서 데드락의 가능성이 없는 경우에만 자원을 할당한다.
  - 시스템 상태가 원래 상태로 돌아올 수 있는 경우에만 자원을 할당한다.
- Deadlock Detection and recovery (데드락 탐지 및 회복)
  - 데드락 발생은 허용하되, 그에 대한 탐지 루틴을 두어 데드락 발견시 회복할 수 있도록 한다.
- Deadlock Ignorance (데드락 무시)
  - 데드락을 시스템이 책임지지 않는다.
  - UNIX를 포함한 대부분의 OS가 채택

</br>
</br>

## Deadlock 예시

> S와 Q가 1로 초기화된 세마포어라고 가정하자.

![image](https://user-images.githubusercontent.com/75905803/204204008-ed87f7da-6cc9-44f2-86ff-cd2f14d8d06c.png)

* P0이 CPU를 얻어서 P(S) 연산까지 수행하여 S 자원을 얻었다고 가정해보자.
* 그런데 여기서 P0의 CPU 할당 시간이 끝나 Context Switch가 발생하여 P1에게 CPU 제어권이 넘어갔다.
* P1은 P(Q)연산을 수행하여 Q 자원을 얻었으나 또다시 CPU 할당 시간이 끝나 Context Switch가 발생하여 P0에게 CPU 제어권이 넘어갔다.
* P0은 P(Q)를 통해 Q 자원을 얻고 싶지만, 이미 Q 자원은 P1이 갖고있는 상태이므로 Q자원을 가져올 수 없다.
* 마찬가지로도 P1도 P(S)연산을 통해 S 자원을 얻고 싶지만 이미 S 자원은 P0이 갖고 있는 상태이므로 S 자원을 가져올 수 없다.
* 이렇게 P0과 P1은 영원히 서로의 자원을 가져올 수 없고, 이러한 상황을 Deadlock이라고 한다.

</br>

> Deadlock 해결 방안

* 자원의 획득 순서를 정해주어 해결하는 방법이 있다.
* S를 획득 해야만 Q를 획득할 수 있게끔 순서를 정해주면 프로세스 A가 S를 획득했을 때 프로세스 B가 Q를 획득할 일이 없다.


</br>
</br>

## Reac Condition

* 스레드가 여러개인 상황에서 코드가 동시에 실행되어 하나의 값에 동시에 접근하는 경우를 말한다.
* 예를 들어 하나의 배열에 여러 스레드가 동시에 접근하게되어 코드가 의도대로 동작하지 않는 경우를 Race Condition이라고 한다.

#

### 왜 Race Condition이 발생할까?

* Thread-Safe하지 않기 때문이다.
* 즉 여러 스레드에서 질서없이 자원에 접근했기 때문이다.

### Race Condition을 방지하려면 어떻게 해야할까?

* 자원에 접근할 때 Serial Queue를 활용하여 `질서를 만들어주어서 해결하는 방법`이 있다.
* 세마포어 등을 활용해서 `접근 가능한 스레드의 수를 제어`해주는 방법도 있다.

</br>
</br>

## 운영체제에서 Race Condition은 언제 발생할까?

> Kernel 수행 중 인터럽트 발생 시

![image](https://user-images.githubusercontent.com/75905803/204204603-5b5fd7dd-47da-4399-85d7-d19807c58b35.png)

* 운영체제(kernel)가 CPU에서 실행 중일 때
* 변수의 값이 1 증가하는 일을 수행할 때, 메모리 변수 값은 CPU register로 불러들이고(`load`) 그 register 값을 1 증가시키고(`increase`), 이 값을 다시 메모리의 변수 위치에 갖다준다. (`store`)
* 위 작업 중 인터럽트가 들어오면, 인터럽트 처리 루틴으로 넘어가며 결과적으론 인터럽트는 반영이 안되고 count++만 처리된다.
* 먼저 하던 일을 끝내고 인터럽트를 처리하여 Race Condition에 대응한다. 즉 순서를 정해주면 된다.

</br>
</br>

> Process가 system call을 하여 kernel mode로 수행 중인데 context switch가 일어나는 경우

![image](https://user-images.githubusercontent.com/75905803/204204954-fb4b6338-db0f-404e-88c6-2c862a55623c.png)

* 두 프로세스의 address space(주소공간) 간에는 data sharing이 없다.
* 그러나 시스템 콜을 하는 동안에는 kernel adress space의 데이터를 access하게 된다. (=share)
* 이 작업 중간에 CPU를 선점해가면 race condition이 발생한다.

![image](https://user-images.githubusercontent.com/75905803/204205055-0419874b-1f1a-4b43-a59f-8a519c207024.png)

* 이를 방지하기 위해 커널 모드에서 수행 중일때 CPU를 선점하지 않는다.
* 즉 커널 모드에서 사용자 모드로 돌아갈 때 선점한다.

</br>
</br>

> Multiprocessor에서 shared memory 내의 kernel data

![image](https://user-images.githubusercontent.com/75905803/204205111-3388179f-d3c9-4b5d-a108-65fcebf70150.png)

* 작업 주체가 여러개인 경우(multiprocessor) interrupt enable/disable로 해결되지 않음
* 방법1
  * 한번에 하나의 CPU만이 커널에 들어갈 수 있게 하는 방법
  * 데이터가 접근할 때 lock을 거는 방법인데 비효율적이다.
* 방법2
  * 커널 내부에 있는 각 공유 데이터에 접근할 때 마다 그 데이터에 대한 lock/unlock을 하는 방법


</br>
</br>

> Process Synchronization 문제

![image](https://user-images.githubusercontent.com/75905803/204205433-a82ba9ec-3572-43b7-baa1-6734f4caf8f5.png)

* 공유 데이터(shared data)의 동시 접근(concurrent acess)은 데이터의 불일치 문제(inconsistency)를 발생시킬 수 있다.
* 일관성(consistency) 유지를 위해서는 협력 프로세스(cooperation process) 간의 실행 순서(orderly execution)를 정해주는 메커니즘이 필요하다.

따라서 Race Condition을 막기 위해서는 concurrent process는 동기화(synchronize)되어야 한다.

---

# Reference

- [반효경 교수님의 운영체제 강의(2014)](https://core.ewha.ac.kr/publicview/C0101020140404144354492628?vmode=f)
