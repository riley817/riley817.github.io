# [TIL & Issue Note] 20230218


## NAT(`Network Address Translation`)를 사용하는 이유
- IPv4 주소체계에서 IP 주소를 절약
- 외부로 패킷이 나갈때는 private IP가 public IP로 바뀌어 전송되므로 내부 IP를 숨실 수 있음(보안)
- 게이트웨이, 라우터, 스위치, 공유기 등

### NAT에서의 문제
1. UDP를 통한 네트워킹을 하기위해서는 서로의 Peer의 목적지를 알아야 한다.
2. 클라이언트 A에서 NAT 거치게 되면 어떤 public IP와 Port 변환될지 모른다.
3. 다른 클라이언트에서 A에게 연결을 하기 위해 public IP는 알고 있지만 private IP와 포트는 알지 못한다.

## UDP Hole Punching
NAT 환경에서 사용자들과 P2P 연결을 중개하는 Relay 서버를 통해 사용자간 직접적인 데이터 전송을 구현

- 각 사용자들의 NAT가 Relay 서버에 접속하면 이 과정에서 각 NAT는 사용자의 공인 IP와 사설 IP를 매핑 테이블을 작성

