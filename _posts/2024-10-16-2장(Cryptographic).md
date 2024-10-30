---
title: 컴퓨터보안 2장 (Cryptographic Tools)
author: JS
date: 2024-10-16 21:00:00 +0900
categories: [Computer Security, Cryptographic Tools]
tags: [computer security, cryptographic, DES, AES, CBC, CFB]
render_with_liquid: false
published: true
---

추가 일시: September 17, 2024 1:24 PM
강의: 컴퓨터보안

# 암호화 방법

## contents

- Symmetric Encryption( 대칭키 암호화)
- Message Confiendtiality
- Public Key Encryption(비대칭키 암호화)
    - =Asymmetric encryption

- **Encryption**: 데이터를 보호하기 위하여 암호화 필요
    - Confidentiality(기밀성), Integrity(무결성, 허용되지 않은 사람은 데이터 수정 X)
- **Message authentication**
    - 무결성은 제공하지만 기밀성은 제공X

### Symmetric Encryption

- 전송 또는 저장된 데이터에 대한 기밀성을 제공하는 보편적인 기술
- key 1개 사용
- 받는 사람과 보내는 사람이 키 공유하고 있어야함→ 어떻게 나눠 가질까??

![img](/assets/img/security/스크린샷%202024-09-18%20120012.png)

- 5개의 재료 필요
    - Plaintext
    - encryption algorithm  (plaintext를 암호화하기 위한 알고리즘)
    - secret key
    - ciphertext(암호화된 text)
    - decryption algoirthm(암호화된 ciphertext를 해독하기 위한 알고리즘)

x와 k를 파라미터로 만든 y

그 y와 k를 다시 파라미터로 복호화 해서 원래의 Plaintext 도출

### Cryptography

- 일반 text를 암호문으로 변환하는데 사용되는 작업 유형
    - substitution(대체)
        - 한 byte를 다른 byte로 바꿔주는 것
    - Tranposition
        - 위치만 바꾸는 것
- 사용되는 키의 개수
    - symmetric
        - 키 1개 사용
    - asymmetric
        - 키 1쌍 사용(2개)
- 일반 텍스트가 처리되는 방식
    - block cipher
        - block 단위로 큰 data 암호화할 때 사용
    - stream ciper
        - 1byte씩 처리할 것인지
        - 작은 데이터를 간헐적으로 암호화할 때 사용

### 대칭 암호화 공격

- Cryptanalytic attack(암호화 공격)
    - 알고리즘의 특성을 악용하여 사용 중인 특정 일반 텍스트 또는 키를 추론하려고 시도
- Brute-Forec Attack
    - 무작위로 다 해보는 것
    - 평균적으로 모든 키의 **절반** 정도를 시도해야 성공할 수 있음
        - Secure Hash Function
    - key 길이에 따라 다름
        
        ![img](/assets/img/security/스크린샷%202024-09-18%20121203.png)
        
        위의 2개는 안전X, 아래 3개는 안전

![img](/assets/img/security/스크린샷%202024-09-18%20121325.png)

**DES(Data Encryption Standard)**: 안전하지 않음, 사용하지 않음

**Triple DES**: DES를 3번 사용해 안전(key size가 56의 3배)

**AES(Advanced Encryption Standard):** 현재 가장 많이 사용

### Feistel Cipher Structure

- 복호화할 때 왼쪽과 오른쪽 바꿔줌→ 똑같은 구조 사용할 수 있음
- 데이터를 두 부분으로 나누어 좌, 우 두 부분에 교대로 비선형 변화을 적용시키는 구조
- trudy가 키를 훔쳐도 function을 알지 못하기 때문에 보안성 좋음

![img](/assets/img/security/스크린샷%202024-09-18%20121714.png)
plaintext를 반으로 쪼갬

Li=R1=L0 XOR f(k1)

Rn=Li=R1

R1=Ln XOR f(k1)=L0

> 같은 값을 2번 XOR하면 원래 값 나옴
> 

### Block Cipher Structure

- round의 순서(round 여러 번 반복)
- 키에 의해 제어되는 대체 및 순열 포함

![img](/assets/img/security/스크린샷%202024-09-18%20122326.png)

## DES(Data Encryption Standard)

- 64bit의 plaintext를 56bit 크기의 key를 이용해서 암호화하는 알고리즘
- key의 길이가 짧아 **Brute-Force**에 약하다.

## Triple DES

- DES 알고리즘을 3번 적용→ **Brute-Force 공격의 취약점 극복**
- 3번 사용하기 때문에 느림
- 64bit block size 사용

![img](/assets/img/security/스크린샷%202024-09-18%20123011.png)

**C=E(K3,D(K2,E(K1,P)))**

왜 E(암호화), D(복호화) 번갈아가면서 사용할까??→ 단순DES 와의 호환 때문이다. 만약 3DES 의 키 K1, K2 를 같은 키를 사용한다면 3DES 는 일반 DES 와 동일하게 된다. 

## AES(Advanced Encryption Standard)

- 가장 널리 사용됨
- 3DES는 장기간 사용에 적합하지 않음
- 안전함, 오랫동안 쓸 수  있음

![img](/assets/img/security/스크린샷%202024-09-18%20123405.png)

encrypt에서 하드웨어 하나 만들면 decrypt 연산에도 사용

**마지막 round만 3개→ mix column 없음**

- mix column
    - 각 열에서 개별적으로 작동
    - 각 바이트를 열에 있는 4바이트 모두의 함수인 새 값에 매핑
    - 유한필드에 대한 방정식 사용
    - 열에서 바이트의 좋은 혼합을 제공하기 위해
- Add round key
    - 확장된 키 비트가 있는 단순한 XOR 상태
    - 라운드 키 확장의 복잡성 및 AES의 다른 단계로부터의 보안

sub bytes와 shift rows 순서 바뀌어도 연산 결과는 같음

- shift Rows
    - 서로 다른 block 이동→ transposition, substitution

### 실용적인 보안 문제

block 단위로 암호화→ 같은 plaintext에서는 같은 cipher text가 나오는 문제점 있음

해결책: mode of operations

### mode of operations

1. Electronic Codebook(ECB)
    1. 같은 키를 사용해서 각각의 64 bit plaintext bit를 암호화
2. Ciper Block Chaining(CBC)
    1. 암호화 알고리즘의 input이 다음 64 bit plaintext와 전의 64 bit cipher text임
    2. block들을 연결해서 사용
    3. **Authentication: 맨 마지막 block에 대한 암호화 가지고 있으면 중간 데이터 바뀌면 최종 암호화 값이 바뀜**
3. Cipher feedback(CFB)
    1. Authentication
    2. 이것도 연결
4. Output Feedback(OFB)
    1. Stream-oriented transmission
5. Counter(CTR)
    1. 첫 번째 counter 값 알면 그 다음 값 알 수 있으므로 병렬 처리 가능
    2. counter 하나 두어서 시작 값은 random하게 함. counter 값 증가시키면서 암호화하고 이 값과 plaintext를 XOR 연산으로 ciphertext 만듬

### ECB

- 가장 간단한 mode
- 일반 텍스트는 한 번에 B bit씩 처리되며 각 블록은 동일한 키를 사용하여 암호화됨
- "codebook"은 각 일반 텍스트 블록에 대해 고유한 암호문 값을 갖기 때문
    - 반복되는 암호문에서 반복되는 일반 텍스트가 표시되므로 긴 메시지에는 안전하지 않음
- 보안 결함을 극복하려면 동일한 일반 텍스트 블록이 반복될 경우 다른 암호문 블록을 생성하는 기술이 필요

### CBC(Cipher Block Chaining)

![img](/assets/img/security/스크린샷%202024-09-18%20125009.png)

1. 처음은 받을게 없기 때문에 IV(initialize vector) 받아서 P1과 XOR해서 나온 값을 다음 input 값으로 넘김
2. 이 C1을 받아서 P2와 XOR해서 값 생성(chain 형태)
3. 마지막 값인 Cn만 잘 저장하고 있으면 중간에 바뀌었는지 확인 가능

**문제점: 1 bit error가 발생하면 뒤에 다 영향을 줌**

### CFB(Cipher Feedback)CBC와 다르게 decrypt 할때도 encrpty 연산 사용

![img](/assets/img/security/스크린샷%202024-09-18%20125949.png)

C1과 암호화된 P2를 XOR 연산

- OFB(plaintext와 XOR 하기 전에 값 전달): P와 무관하게 암호화 연산 가능

### Counter(CTR)

![img](/assets/img/security/스크린샷%202024-09-18%20130232.png)

병렬적으로 처리

## Block Cipher Encryption&Stream Encryption

![img](/assets/img/security/스크린샷%202024-09-18%20130402.png)

block cipher encryption: block 단위로 나눔

Stream Encryption: 1 byte씩 random한 sequence 만듬

### Block & Stream Ciphers

- block cipher
    - key 재사용 가능
- steram cipher
    - 연속적으로 실행
    - 한 번에 하나의 요소 출력
    - **빠르고 적은 코드 사용**
    - 한 번에 한 byte의 plaintext를 암호화한다.

### 속도 비교

![img](/assets/img/security/스크린샷%202024-09-30%20121532.png)

3DES: 속도느림

RC4: AES 보다 빠름

### Message Authentication

message에 대해 공격자가 변조하는 것을 방어

![img](/assets/img/security/스크린샷%202024-09-30%20121730.png)

message와 key 값을 사용해 MAC(Message Authentication Code)을 만들고 MAC을 원래의 message와 같이 보냄→ 같은지 compare해서 바뀌었는지 확인

> 쌍방의 통신 장치가 공통의 secret key K를 공유한다고 가정
> 
- 발신자는 MAC과 함께 메세지를 전달
- 수신자는 수신된 메시지와 공유 비밀 키 K를 MAC 알고리즘에 입력하고 MAC값을 다시 계산함
- 새로 계산된 MAC과 발신자로부터 수신된 MAC이 같은지 확인
- → 일치하면 수신자는 메시지를 수락하고 일치하지 않으면 변경되었을 수 도 있으므로 메시지가 안전하지 않다고 생각

### Secure Hash Functions

![img](/assets/img/security/스크린샷%202024-09-30%20134019.png)

- Hash 함수: 임의의 길이 데이터를 고정된 길이의 데이터로 매핑하는 수

L bits: input(임의의 길이의 데이터)

Hash value h: 128 bit, 256 bit→ 가능한 hash 표현 2^128, 2^256

fixed length output을 줌(고정 길이 출력)

### Message Authentication Using a One-way Hash Function

![img](/assets/img/security/스크린샷%202024-09-30%20134754.png)

보낸 message가 바뀌지 않은 걸 확인해줌

- Using Conventional encryption
    - MAC을 decrypt해서 비교
    - 비교해서 일치하면 변조X
- Using public-key encryption
    - 한 쌍의 key 사용
    - private key로 암호화
    - public key로 복호화
- Using secret value

### Hash Function Requirements

- 모든 크기의 데이터 블록에 적용 가능(L bit)→ 종류: 2^L개
- 고정 길이 출력(H bit)→ 종류: 2^H개
    - L>H이므로 input 개수>output 개수⇒ 같은 output값 발생**(=Collision)**
- x라는 값을 이용해 H(x) 쉽게 도출
- But H(x)값을 알 때, x 값 아는 것은 어려움
- Weak Collision
    - H(y)=H(x)일 때, 서로 다른 값(x ≠ y)를 찾는게 어려워야 한다.
- Strong Collision
    - random한 값에 대해서도 hash 같은 쌍(H(x)=H(y)) 찾아내는게 어렵다.

### Security of Hash Function

- **SHA(Secure Hash Algorithm): 가장 널리 알려진 hash 알고리즘**
1. cryptanalysis
    1. 알고리즘의 논리적 약점 활용
2. brute-force attack
    1. 해시 함수의 강도는 알고리즘에 의해 생성된 해시 코드의 길이에 의존한다.
3. password
    1. 암호의 해시는 운영체제에 의해 저장됨
4. intrusion detection(침입 탐지 시스템)
    1. 시스템의 각 파일에 대해 H(F)를 저장하고 해시 값을 보호
    2. 해시 값을 비교해서 변조 여부 확인

### SHA

![img](/assets/img/security/스크린샷%202024-09-30%20140633.png)

1. 모든 사이즈는 bit로 측정된다.
2. birthday attack: n명의 사람들 모였을 때, 생일 같은 사람 확률(50% 넘을지)
    1. 1-(365/365*364/365*363/365 ….)<50 인가???
    2. 23번째일때!! 생일 같을 확률 50% 넘음
    3. 365=2^8.xxx니깐 23=2^4.xxx이므로 ⇒ **2^n/2**가 된다.
3. 2^n/2 번 시도하면 collision 발견 가능

### HMAC Design Objective

- 수정 없이 사용
- 분석 쉬워야함

### HMAC 구조

![img](/assets/img/security/스크린샷%202024-09-30%20141223.png)

Message와 key 받아서 Hash값 만들어내는 것

- ipad: 입력 패딩(00110110) 패턴이 반복
- opad: 출력 패딩(01011100) 패턴 반복
- K+: 키 값
1. K+와 ipad를 XOR 연산해서 message와 합친다.
2. 초기화 벡터와 합친걸 hash함수를 돌린다
3. 이 결과값과 K+와 opad를 XOR연산해서 나온 갑을 결합한다.
4. 이걸 다시 hash함수를 돌려서 HMAC 도출

### Security of HMAC

## Public Key Encryption

- 한 쌍의 key 사용

![img](/assets/img/security/스크린샷%202024-09-30%20143151.png)

- plaintext
    - 알고리즘에 입력으로 공급되는 읽을 수 있는 메시지 또는 데이터
- encryption algorithm
    - 일반 텍스트에서 변형을 수행
- public and private key
    - 하나는 암호화용, 하나는 복호화용
- ciphertext
    - scrambled message가 출력으로 생성됨
- decryption algorithm
    - 원본 일반 텍스트를 생성

> 공격자는 ciphertext 복호화할 수 있다.(Bob의 public key 알 수 있다면)
> 

### Applications for Public-key Cryptosystem

![img](/assets/img/security/스크린샷%202024-09-30%20143539.png)

- Elliptic Curve Cryptography(ECC): RSA에 비해 key size 작음→ 성능 좋음, 계산량 줄임

### 1. RSA

- encrypt
    - C= M^e mod n
    - M=C^d mod n=(M^e)^d mod n=M
    - 발송자와 수신자 모두 n과 e 알고 있음(public key)
    - 발송자만 private key(d) 알고있음
    - public key PU={e,n}
    - private key PR={d,n}

![img](/assets/img/security/스크린샷%202024-09-30%20144039.png)

EX) 

![img](/assets/img/security/스크린샷%202024-09-30%20144214.png)

1. plaintext가 88이다(M=88)
2. 소수인 p, q 선택: p=11, q=17
3. n=p*q이므로 n=187
4. o(n)=(p-1)(q-1)=160
5. gcd(160,e)=1: 160과 최대공약수가 1인 공개키(e) 선택→ e=7
6. **C=M^e (mod n)=88^7 mod 187 = 11**

복호화

1. de mod 160 =1→ d*7 mod 160 =1 인 d는 23
2. **M=C^d (mod n)=11^23 mod 187=88**

### Security of RSA

1. brute force
    1. 모든 가능한 private key 시도
    2. 방어는 큰 key 공간을 사용하지만 실행속도 느려짐
2. mathematical attack
    1. n값 주어졌을 때 p,q 알아내면 됨→ 소인수분해
3. Timing Attack
    1. 암호해독 알고리즘의 실행시간에 따라 다름
    2. 전력사용량 계산해서 깸

### Progress in Factorization

- 수학적으로 깨기 위해서 소인수분해 BUt 어려움

## 2. Diffie-Hellman Key Exchange

- 후속 메시지 암호화에 사용할 수 있는 비밀키를 안전하게 교환하는 실용적인 방법
- 불연속 로그 계산의 어려움에 의존

![img](/assets/img/security/스크린샷%202024-09-30%20151917.png)

예제)

![img](/assets/img/security/스크린샷%202024-09-30%20152418.png)

## 3. Digital Signature

- private key를 이용해 hash code를 암호화하여 생성
- 기밀성 보장X

![img](/assets/img/security/스크린샷%202024-09-30%20153726.png)

- key distribution
    1. key는 A에 의해 선택되어지고, B에게 물리적으로 전달됨
    2. 믿을 수 있는 제 3자가 전달
    3. 이전에 암호화 통신 하고 있었고 새로운 key 만들어서 보낼 수 있지만 key가 공격자에게 노출되면 새로운 key도 노출될 가능성 있다.
    4. 제 3자가 key 만들어서 A,B 전달해줌

![img](/assets/img/security/스크린샷%202024-09-30%20154206.png)
‘A가 B와 통신하고 싶다’고 요청하면 KDC(Key Distribution Center)는 A, B 각각 암호화된 채널 이미 유지하고 있기 때문에(로그인) KDC가 random한 key 만들어서 A, B에 전달하면 A와 B는 암호화된 통신 가능

### Kerberos

- 티켓 기반의 컴퓨터 네트워크 인증 프로토콜
- 사용자와 서버가 서로 식별할 수 있는 상호인증을 제공
- 티켓: 유저 아이디를 안전하게 전달하는데 사용되는 정보 패킷(유저 아이디, 호스트 ip, 타임스탬프, 티켓 수명, 세션키)
- TGS: SS(Service Server)와 통신하기 위한 티켓을 유저에게 발급해주는 서비스
- SS: 유저가 최종적으로 통신하고자 하는 목적지 서버
- 세션 키: 유저와 서비스 간의 통신에 필요한 키
- 

![img](/assets/img/security/스크린샷%202024-09-30%20154908.png)

1. 유저는 자신의 아이디를 AS(Authentication Server)에 보내서 TGT를 요청함
2. AS는 전송받은 아이디가 AS의 데이터베이스에 있는지 확인하고, 있다면 두 가지 정보를 생성해 유저에게 보내줌
    1. 요청을 보낸 유저의 패스워드를 통해 만든 키로 암호화한 TGS 세션키
    2. TGT(티켓을 받기 위한 티켓)
3. 유저는 AS로부터 받은 두 가지 정보를 통해 TGS 세션 키를 얻고, Authenticator(유저 아이디와 타임스탬프를 TGS 세션 키로 암호화 한 데이터)를 만들어 TGT와 함께 TGS에 보내준다
4. TGT는 티켓과 Authenticator를 복호화하여 유저 아이디가 일치하는지 확인하고, 일치한다면 유저에게 티켓과 SS 세션 키를 발급해준다.
5. 유저는 TGS로부터 암호화된 SS 세션 키와 티켓을 받고, 암호화된 SS 세션 키를 복호화하여 또 다른 Authenticator를 만들어 티켓과 함께 SS에게 보내준다.
6. SS는 유저로부터 받은 Authenticator와 티켓을 복호화하여 유저 아이디가 일치하는지 확인한 후, 일치한다면 Authenticator에 들어 있던 타임 스탬프를 SS 세션 키로 암호화하여 유저에게 보내줌
7. 유저는 답신 받은 데이터를 SS세션 키로 복호화하여 앞서 SS에게 보낸 타임 스탬프와 일치하는지의 여부 체크
- 단점
    - 서버가 다운 될 경우, 새로운 유저는 로그인할 수 없다.
    - 여러 개의 서버를 운용하는 등 서버가 작동하지 않을 때를 대비할 수 있는 메커니즘을 구현해야함
    
### Certificate Authority(CA) 인증서 발행기관
- public key + User ID
- Certification Revocation List(CRL)
    - 무효화된 인증서 관리
    - 유효기간 남아있지만 취소한 인증서 정보 따로 보관
- 사용자는 안전한 방식으로 자신의 공개 키를 기관에 제시하고 인증서를 얻을 수 있음
![img](/assets/img/security/화면%20캡처%202024-10-30%20144827.png)


### Rotor Machine
- 매우 복잡하고 다양한 치환 암호 구현
- 일련의 실린더 사용하며 각 문자가 암호화된 후 회전하고 변경됨
- 한 글자 누를 때마다 바뀜-> fast rotor가 1칸 아래로 움직임
    ![img](/assets/img/security/화면%20캡처%202024-10-30%20145729.png)
