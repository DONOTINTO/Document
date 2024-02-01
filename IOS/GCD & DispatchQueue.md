# GCD (Grand Central Dispatch)
IOS에서 GCD는 멀티코어 시스템에서 동시성 실행을 제공하는 언어 요소, 런타임 라이브러리라고 소개한다.
Dispatch Queue는 이런 GCD의 동시성 프로그래밍을 제공하는 스위프트의 GCD 기반 작업관리 객체이다.

# DispatchQueue

<img width="928" alt="스크린샷 2024-02-01 오후 9 19 14" src="https://github.com/DONOTINTO/Document/assets/123792519/0de40f3f-5768-4b50-a9cc-1f7f0c6be9d2">

조금 더 명확하게 보자면, 디스패치 큐는 앱의 메인 스레드 또는 백그라운드 스레드에서 작업을 serially(하나의 스레드) 또는 concurrently(여러 스레드)로 처리하도록 관리하는 객체이다.

## DispatchQueue의 역할

디스패치 큐는 위의 설명대로 동시 작업을 관리해주는 객체이다.
즉, 작업들을 어떻게 분배하고 처리할지 결정해주는 작업 반장님이다.

디스패치 큐에서 작업을 분배하는 방식은 다음 두가지이다.
- serially(main thread)
- concurrently(global thread)

---

### 분배 방식

위 개념을 이해하기 앞서 CPU에는 크게 Main Thread와 Global Thread로 나뉘어지는데, 어플을 실행해서 동작하는 모든 작업 실행들은 Main Thread에서 처리하게 된다.

하지만 Main에서 모든 작업을 처리하면 효율적으로 작업을 처리할 수 없을 것이다. CPU에는 Main Thread뿐만 아니라 다른 Thread들도 존재하기 때문인데, Main Thread를 제외한 나머지 Thread들은 모두 Global Thread로 구분된다.

---
여기서 serially는 Main Thread를 concurrently는 Global Thread를 의미한다고 생각하면 이해하기가 쉽다.

> serially

![Frame 58](https://github.com/DONOTINTO/Document/assets/123792519/95e1faec-3b58-480b-be2d-fb9a10619e14)


serially는 순차적으로라는 뜻으로 Main Thread만 사용하기 때문에 작업이 순차적으로 이루어진다고 이해할 수 있다.

> concurrently

![Frame 57](https://github.com/DONOTINTO/Document/assets/123792519/12652ae9-7e44-48d6-a133-e5eb3afe2283)


반대로 concurrently는 동시에라는 뜻으로 여러 Global Thread에 작업을 분배하여 동시적으로 이루진다고 이해 할 수 있다.

---
그렇다면, 이렇게 작업의 분배 방식만이 디스패치 큐의 역할이 끝일까?   
위에서 분배하고 처리를 해주는 방식을 결정해준다고 했듯 디스패치 큐는 이를 다시 순차적으로 실행할 것인지 동시에 실행할 것인지를 결정해준다.

### 처리 방식

디스패치 큐는 어떻게 분배할지를 결정했으면 어떻게 처리해야할지도 결정해줘야 한다.

처리 방식은 다음과 같이 또 구분된다.

- sync
- async

> sync

순차적으로 작업을 처리하고 싶다면 sync를 사용한다.
디스패치 큐에 sync로 Task를 보내면 Task를 보낸 스레드는 해당 Task가 완료될 때 까지 Block(정지)된다.

Task가 완료되면, Block이 해제되면서 다음 Task가 동작하게 된다.

> async

순서를 기다리지 않고 작업을 처리하는 방식이 async이다.   
async는 Task가 완료의 기다리지 않고 바로 이어서 다음 작업을 진행한다.

## 4가지 동작
- main + sync   
메인 스레드에서 순차적으로 작업을 진행

- main + async   
메인 스레드에서 순서를 지키지 않고 작업을 진행

- global + sync   
여러 스레드에서 순서를 순차적으로 작업을 진행

- global + async   
여러 스레드에서 순서를 지키지 않고 작업을 진행

## 데드락(Dead Lock)
main.sync는 데드락이 걸리기 매우 쉬운 상황이다.
sync는 Task를 디스패치큐에 보내서 처리가 끝날 때까지 해당 스레드는 Block이 된다고 했다.

우선 기본적으로 모든 Task는 main 스레드에서 이루어진다. 즉 main.sync는 main에서 main으로 다시 Task를 분담하는 것이다.

![Frame 60](https://github.com/DONOTINTO/Document/assets/123792519/6c15f047-eabf-4444-aef5-941497de61ec)


1. Main Thread는 DispatchQueue로 Task를 전달
2. 전달과 동시에 Main은 Task의 작업 완료를 대기
3. DispatchQueue는 전달받은 Task를 다시 Main이 수행하도록 작업 분배
4. Main Thread는 Task의 완료를 대기(Block)중이라 DispatchQueue에게 Task를 받을 수가 없음 
5. Dispatch Queue는 Main Thread의 Block이 해제되기만을 대기

위로 인해 Main Thread와 DispatchQueue는 서로의 작업이 완료되기만을 무한히 대기하게 된다.

이러한 상황은 데드락(Dead Lock) 상황이라고 한다.

그렇다고 해서 main.sync가 항상 데드락이 발생하는 것은 아니라고 한다.   
다양한 상황들이 있다고 하지만 아직은 그 부분까지는 모르겠다.
