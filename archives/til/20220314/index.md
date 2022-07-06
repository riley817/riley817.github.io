# [TIL & Issue Note] 20220314

# 블록체인 원리(2) - 블록의 구조

## 1. 블록의 이해
### 블록 (Block)
- 의미 있는 묶음 = 1 BLOCK
- 어떤 의미있는 단위를 한 블록으로 정의
- **특정 데이터를 다루는 단위**

#### 비트코인 블록체인
- 1 MB 까지의 한도 내에서 거래내역을 기록한 단위 (2000 ~ 3000 거래 내역)
- 비트코인 캐시 (8MB 까지 허용), 비트코인 골드

#### 이더리움 블록체인
- 이론적인 한도는 없음

#### 비트코인 블록의 구조
- **블록 헤더** : 데이터의 요약 정보 
	- 블록 헤더의 크기 : 80 Byte
	- 항상 일정한 크기를 유지
- **블록 데이터** : 실제의 데이터를 담는 부분

## 2. 블록의 구성요소

### 블록체인의 구성 요소
- **블록 크기** : 4 Byte
- **거래내역 개수** : 가변 (1 ~ 9 Byte)
- **트랜잭션** : 가변
	- 트랜잭션 크기는 기록된 트랜잭션 개수에 따라 변화

#### 블록체인의 특징 1.
- 실제 거래내역 개수가 적을 때 실제로 개수를 저장

#### 블록체인의 특징 2.
- 거래내역 개수가 많아질 때는 바이트를 사용

#### 거래내역 개수의 규칙 
- 1 ~ 9 Byte로 제한
- 선두 바이트 : 거래 개수에 적혀 있는 바이트 표시 -> 전체 데이터 바이트 절약
- 거래내역 개수 = 트랜잭션 개수

### 비트코인 트랜잭션
- 하나의 비트코인 주소로부터 다른 비트코인의 주소로 거래
- 하나의 비트코인 주소로부터 다른 하나의 비트코인의 주소로 거래
- 하나의 비트코인 주소로부터 다른 다수의 비트코인 주소로 거래
- 기록 개수에 대한 제한 없음
- 수령자가 늘 수록 더 많은 저장 공간 필요
- 하나의 거래의 최소 크기 166Byte -> 실제 거래내역 크기는 300 Byte 초과 ( 거스름돈, 다수의 수령자)
- 이론적으로 5,000 ~ 6,000 개의 거래내역 포함 -> 실제 거래내역은 3,000 개 미만 

## 3. 블록헤더의 구성요소

### 블록헤더의 구성 요소
- 버전 정보 - 4byte
- 이전 블록 해시 값 - 32 byte
- 머클트리 루트 - 32 byte
- 타임스탬프 - 4 byte
- 타깃 난이도 - 4 byte
- 난스 - 4 byte


#### 버전 정보 (4 Byte)
- 블록을 만들 당시의 소프트웨어 버전을 의미
- **=법령정보** -> 버전에 따라 서로 다른 규칙 적용 -> 블록 무결성의 확인 부분이 상이
- 시간의 흐름에 따른 법의 변화 -> 소프트웨어 버전에 따라 블록 생성의 규칙이 변화
- 어떤 규칙을 사용해 **블록을 검증할지** 알려주는 값

#### 이전 블록 해시 값 (32Byte)
- 현재의 블록에 **이전 블록의 데이터 값**이 포함
- SHA256 32 Byte 해시 값
- 일련의 과정을 통해 이전 블록에서 해시 값을 생성
- 생성된 해시 값이 현재 블록에도 기록
- **모든 블록은 자기의 특정 값을 가지며 다음 블록에 전달**

##### 해시 값 기록의 이유
- 이전 기록과 비교로 **변조 여부 확인**
- 블록의 어떤 값이 변경되었음을 손쉽게 탐지

#### 머클트리 루트 (32 Byte)
- 블록 데이터에 담겨 있는 모든 트랜잭션의 요약 정보
- 블록 데이터에 모든 트랜잭션 -> 하나의 해시 값으로 요약
- 1 MB에 달하는 3,000 개의 트랜잭션 요약
- 블록 데이터에 모든 트랜잭션 -> 단 32 Byte의 해시 값 -> 머클트리 루트에 데이터로 저장

#### 타임스탬프 (4 Byte)
- 블록이 만들어진 시간 기록
- 블록의 생성 시간을 파악 가능


 #### 타깃 난이도 비트 (4 Byte)
 - 블록 생성의 난이도에 대한 값을 저장하는 비트
 - 일련의 공식을 통해 32 Byte 로 변환

#### 넌스 (Nonce, 4byte)
- 해시 퍼즐의 정답
- 해시 퍼즐을 가장 먼저 해결한 노드가 요청서를 기록
- 해시 퍼즐의 정답 -> 넌스를 찾는 과정
- 모든 가능한 조합을 통한 무차별 대입법
- 무수한 계산을 통해 정답을 찾은 정수

### 블록체인 마다 서로 다른 정의 가능
#### 비트코인 헤더의 크기  
- 헤더의 크기는 80 Byte 고정
- 6개의 필드

 #### 이더리움 헤더
 - 최소 508 Byte로 가변적
 - 15개의 필드


 ### 블록헤더
 - 실제 블록에 담겨 있는 모든 데이터에 대한 정밀한 요약
 - 블록 데이터의 기능에 따라서 서로 다르게 정의

 # 블록체인 원리 (3) - 작업증명 : 해시 퍼즐과 난이도

## 1. 블록만들기와 해시 퍼즐

### 해시 퍼즐
- 무차별 대입법 -> 특정 값 -> 무한 반복 산수 문제
- 해시 퍼즐을 푸는 수학적 공식은 없음
- 무수히 반복되는 산수 문제 해결
- 해시를 통해 얻게 되는 고유한 값 32 Byte 값 -> SHA-256
**- 유효한 블록의 해시값을 찾을 때까지 무한 반복**
- 해시 퍼즐 정답 = 블록 고유 해시값
- 제네시스 블록 해시값 = 2의 32승 번 값 계산 -> 약 10분동안 계산


### 해시 퍼즐 풀이
1. 블록 헤더의 넌스값을 0 으로 설정
2. 비트코인 해시 함수를 연속 2번 해싱 (SHA-256) : 임의의 32 Byte 값
3. 블록 전체를 해시함수를 연속 두 번 적용하여 나온 해시값 H와 주어진 목표값 T 값을 비교
4. 주어진 값보다 더 작거나 갖지 않다 -> 넌스 1증가 (1 ~ 32 까지 증가) -> 임의의 32 Byte 값 생성 반복
5. 판단박스 목표 값보다 T보다 작아질 때까지 반복

### 넌스값
- 입력의 사소한 변화
- 32 Byte 충분히 작은 값이 나올 때까지 반복
- T(목표값) -> 목표값이 작을 수록 만족 힘듦

### 작업증명
- 퍼즐을 이용해서 의도적으로 막대한 에너지를 소모하게 만든 방식
- 스팸 방지를 위해 개발
- 의도적으로 막대한 에너지를 소모하게 하여 그 일을 억제
- 최초 기록 및 변경에 막대한 에너지 소모 -> 반대 급부의 경제적 이득이 없으면 변경 이유 없다 
- 최초의 기록된 노드의 검증 -> 합당한 이유 없다면 정상적 기록을 통한 보상금 -> 합리적 선택

#### 스팸 방지
- 네트워크 과부하 문제
- 어떤 작업을 반드시 해야만 되도록 부과 -> 스팸 억제

## 2. 해시 퍼즐과 난이도

### 해시 퍼즐과 난이도
- 목표값이 작아지면 해시퍼즐을 찾을 확률이 줄어듬
- 목표값을 성취하려면 과정 반복 필요

### 해시 퍼즐의 비유
- 더 작은 해시 값 찾기 = 목표 값 찾기

#### 해시 함수를 통해 얻는 값
- 32 byte로 이루어진 값이 어떤 특정 패턴을 이룰 수 있는 방법 -> 0000 (0), 1111 (2의32승-1)
- 목표값은 작아질 수록 ㅎ시행횟수 증가 -> 오래시간 소요
- 목표값을 조정하여 목표값에 맞는 해시값을 찾는데 필요한 계산횟수
- 횟수는 오래 걸리지만 시간은 10분 소요 -> 과거에 비해 빠른 컴퓨터

#### 난이도 조절 이유
- 컴퓨터가 발달 하여도 10분에 하나씩 블록 생성

#### 목표값과 해시값의 관계
- 1,209,600초 -> 10분이하로 블록 생성
- 늦은 시간 -> 하나의 블록생성시 10분 이상 걸림
- 원래 소요시간보다 적을 겨우 -> 난이도 상승
- 원래 소요시간보다 낮음 -> 난이도 하향

### 목표값

### 무어의 법칙
- 반도체의 발달 속도 - 18 개월마다 반도체의 직접도가 두배가 되어
- 18개월마다 컴퓨터 속도 2배 항샹
- **지속적으로 난이도조절**
- 2009 ~ 2017 63배

#### 무어의 법칙에 따른 자연적인 증가
- 막대한 하드웨어의 투자 + 전용 기계 -> 인위적 수치
- **난이도 증가는 블록체인의 영속성에 영향**