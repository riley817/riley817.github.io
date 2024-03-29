# [TIL & Issue Note] 20230218


## Deterministic 방식
- 같은 인풋을 가지고 있으면 두 개의 상태`state`가 동일`equal`하다 (=sync가 맞는다.)
- 서로의 입력을 전송하는데에 시간이 걸린다.
- 얼마나 latency를 잘 극복하는지 desync가 일어나지 않게 만드는 것이 핵심이다.
- desync를 막기 위해 **delay** 와 **rollback** 방식을 활용한다.

### Delay
- 지연시간만큼 입력을 딜레이 시켜서 처리하면 싱크가 맞게 되는 원리
- 쉽고 단순하게 구현할 수 있다.

**문제점**
- 지연시간이 발생한다.
- 지연시간이 보통 100ms 가 넘어가면 플레이어가 이를 감지 할 수 있다.

### Rollback
- 지연시간만큼 다시 시간을 되돌리고 다시 앞감기를 한다.

**문제점**
- 유저 입장에서는 프레임이 갑자기 바뀌어서 렉이 걸린것 처럼 보여짐
- 만들기가 매우 까다롭다
    - 앞감기, 뒤감기, 새로운 입력 끼워넣기를 구현해야 한다.
    - 이전 모든 플레이를 기록하고 있어야 하기 때문에 메모리사용과 복잡도가 올라간다.
    - 플레이어가 많을 수록 각 캐릭터의 모든 상태를 기록하기가 어렵다.

> 보통은 Delay 와 Rollback 을 합쳐서 활용한다. Rollback은 제한적으로 사용함으로써 되도록 플레이어가 알아차릴 수 없게 구현한다.
> ex) 오버워치 - 트레이시 캐릭터 (캐릭터 능력 중 시간을 되돌리는 능력이 있음 이는 rollback 에 부가적인 기능을 활용한 사례 ) 

## Deterministic 방식의 문제점
- 중간 접속 처리가 어렵다. 
- 상태가 아닌 모든 플레이어의 입력을 공유하므로 재접속 시 그동안 모든 입력을 받은 후 싱크를 맞춤
- reconnect 방식이 복잡 
- desync의 상태를 맞추기가 어렵다
- 좋은 방식이고 많은 게임에서 사용하고 있으나 구현이 어렵다.

## 중계서버를 이용하는 방식 - Relay, Broadcast Server
- 서버가 클라이언트에서 받은 패킷을 다른 클라이언트에 전달한다. (내가 보낸 패킷도 받게됨)
- 모든 인풋을 서버에 의존한다.
- 모든 클라이언트는 같은 입력을 받으므로 deterministic 방식이다.
- **서버에서는 프레임을 일정단위로 쪼개고 모든 클라이언트로부터 입력을 게더링한 뒤 일정시간이 지나면 클라이언트에 전송**
- 턴제 방식과 동일하다.
- 구현하기도 쉽고 desync도 발생하지 않는다.

**문제점**
- 서버를 거쳐서 네트워킹이 발생하므로 딜레이가 발생한다.

## 정리
|Rollback|중계서버|
|------|---|
|반응성이 높다|반응성이 낮다|
|desync가 발생한다.|desync가 발생하지 않는다.|
|격투, FPS 등에 적합|AOS, 스포츠 게임에 적합|

## Rollback 방식을 해결하기 위한 방법
- 다른 플레이어로부터 새로운 인풋이 왔을때 특정 시간으로 롤백하는 기술은 매우 복잡하고 버그가 많음
- 매 시간 지정한 프레임만큼 계속 롤백을 처리한다. 
- 중간에 다른 플레이어의 요청이 올경우 롤백 후 끼워넣는다.
- 롤백의 경우 그래픽 변화는 많지 않기 때문에 성능상에 크게 문제가 있지는 않음

## 정리
- **Deterministic** : 같은 input 이면 같은 결과를 받는 것
- input 의 싱크를 맞추는 기법
    - Delay && Rollback
    - Relay 서버
    - 매 프레임마다 rollback

## P2P (Peer to Peer)
클라이언트 끼리 직접 연결하는 방식
- 특정 클라이언트가 방장인 경우
    - 방장을 중심으로 네트워킹이 발생하며 게임의 검증 및 판단을 방장이 한다.
    - 방장이 나가거나 커넥션이 끊긴 경우 게임이 종료된다. 혹은 새로운 클라이언트에 방을 위임한다.
    - 방장의 커넥션이 끊겼을 경우 문제가 발생한다.
- 모든 클라이언트 끼리 연결
    - 각 클라이언트가 다른 클라이언트에 대한 커넥션을 가지고 있다.
    - 각 클라이언트가 관리해야 하는 커넥션 수가 많다.
    - 한명의 커넥션이 유실되도 전체 게임에는 영향을 주지 않는다.

## Relay 서버
- 서버를 두고 클라이언트들이 접속하는 방식
- 서버가 다운되었을 때 게임 자체를 플레이할 수 없는 문제 발생


