# TCP/IP 5계층
---
## TCP/IP 5계층이란?
- OSI 7 Layer가 데이터 통신 과정을 7개 계층(Layer)로 분류한 이론적인 모델이면, TCP/IP 5계층은 데이터 통신에 실질적으로 사용되는 프로토콜 스택이다. 근래 4계층에서 5계층으로 바뀌었다고 한다.

![](https://velog.velcdn.com/images/donotinto/post/0a82b4dd-e7d3-4977-af49-ab6fccd7dc4c/image.png)

> TCP/IP(suite of protocols that specify communications standards)는 프로토콜 모음을 계층화했다는 의미라고 한다. 근데 왜 이름이 약자가 아니라 TCP와 IP를 묶어서 칭하나 했더니 프로토콜 모음들 중 가장 대표적인 프로토콜(중심적인)이라 그렇게 이름 붙여졌다고 한다.

---
## TCP/IP 계층 별 특징
### Application Layer
- 프로그램 구현체(응용 프로그램)과 사용자 인터페이스를 의미한다.
- OS 제공하는 4 L API를 활용해 통신 프로그램이 구현된다.

### Transport Layer
- **Port번호**를 이용하여 최종 도착지인 프로세스(실행중인 응용프로그램)까지 데이터를 전달한다.
- OS 커널에 구현되어 있다.

> **Port번호**는 2byte로 총 16개 bit로 구성되어 있으며,
Well-Known(1~1000)번  포트는 주요 프로토콜에서 사용중이어서, 사용자 어플리케이션은 그외의 번호를 이용한다.

### Internet Layer
- 라우팅을 통해 목적지 IP 주소(IP Address)로 패킷을 전달한다.

> IP Address(Internet Protocol Address)는 Host의 논리적 주소로 IPv4(4byte), IPv6(8Byte)방식으로 존재한다.  

### Data-Link Layer
- 목적지 MAC 주소까지 프레임(== 패킷)을 전달한다.

### Physical Layer
- Encoding을 통해 0과 1의 나열을 아날로그 신호로 변환해서 전송한다.
- Decoding을 통해 아날로그 신호를 0과 1로 해석한다.


---
## TCP/IP 분류

- 총 4개의 계층은 다시 Application(application layer)과 System(Trasport Layer/Internet Layer/Data Link Layer/Physical Layer)으로 구분할 수 있다.


- Application
    - 어플리케이션 레벨에서 구현/관리
    - 네트워크 기능을 사용하는데 목적

- System
    - 하드웨어/펌웨어 , OS 레벨에서 구현/관리
    - 네트워크 기능을 지원하는데 목적
    
## 참고
[jwkim님 블로그](https://velog.io/@jwkim/cs-nw-osi-tcp-ip)   
[ksi05503님 블로그](https://velog.io/@ksi05503/tcp-ip)