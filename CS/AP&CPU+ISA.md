# AP & CPU + ISA
### 목차
1. [개요](#1-개요)
2. [AP와 CPU란?](#2-ap란)
3. [AP와 CPU의 차이점](#3-cpu와-ap의-차이점)
4. [AP와 CPU의 대표적인 종류](#4-ap의-대표적인-종류)
5. [ISA - RISC, CSIC](#5-isa---risc-csic)
6. [참고 문헌](#6-참고-문헌)
---
> #### 1. 개요
<br/>
AP와 CPU가 무엇이고 그 차이와 종류를 정리  
<br/>
<br/>

> #### 2. AP와 CPU란?  

<br/>
AP란 Application Processor의 약자로, 모바일 기기나 태블릿 컴퓨터와 같은 장치에서 응용 프로그램 실행과 관련된 작업을 수행하는 반도체 칩이다.  

<br/>  
CPU란 Central Processing Unit의 약자로, 컴퓨터의 중앙처리장치이다.  
명렁어의 해석과 실행, 연산을 수행한다.  
<br/>
<br/>

> #### 3. AP와 CPU의 차이점

<br/>
AP는 CPU와 다르게 보다 통합된 형태로 제공된다.  
데스크탑에서는 CPU와 GPU, 메모리 기타 요소들이 따로 따로 존재하지만, AP에서는 이를 하나의 칩 안에서 모두 관리한다.

<br/>
이처럼 통합된 하나의 칩 형태로 제공되다 보니 AP 내부에서 데이터 전송에 필요한 추가적인 전력 손실이 줄어들고, 성능을 중시하는 데스크탑과는 다르게 성능 + 배터리 성능까지 중요시하는 AP는 전력 소모를 최적화하는 기능들이 강화되어 있다.

<br/>

**설계 방식** 
- AP  
   - AP는 주로 `RISC(Reduced Instruction Set Computing) 아키텍처`를 기반으로 설계되어 있으며, 이는 명령어의 수를 줄이고 실행 속도를 높이는 방식으로 동작한다.  
   
   - `ARM 아키텍처`가 RISC로 만들어진 대표적인 AP이며, 맥의 M 시리즈 AP 칩은 ARM 아키텍처를 기반으로 만든 `Apple Silicon`이다.  
   
   - 주로 모바일 기기에 사용된다.

   - RISC 아키텍처 -> ARM 아키텍처 -> 여러 AP들 (Snapdragon, Exynos, Apple Silicon)
<br/>
- CPU  
   - CPU는 주로 `CISC(Complex Instruction Set Computer)`를 기반으로 설계되어 있으며, 복잡한 명령어 집합을 가지기 떄문에 하나의 명령어로 여러 개의 기능을 수행할 수 있다.  
   
   - 프로그래밍이 간편하고 메모리 접근이 적어진다.

   - 주로 데스크탑에서 사용된다.

   - CISC 아키텍처 -> x86, x64 -> 여러 CPU들 (intel, amd)
<br/>
<br/>

> #### 4. AP와 CPU의 대표적인 종류
<br/>

- AP
   - Qualcomm의 Snapdragon 
   - Samsung의 Exynos
   - Apple의 A 시리즈
   - Apple의 M 시리즈 (고성능 데스크탑 용)
   - MediaTek의 Dimensity

- CPU
   - Intel
   - AMD
<br/>
<br/>

> #### 5. ISA - RISC, CSIC
<br/>

위에서 RISC 아키텍처, CSIC 아키텍처가 나왔지만 정확히 이게 무엇인지 간단하게 정리해보려 한다.

RISC, CSIC는 ISA(Instruction Set Architecture)의 종류 중 하나이다.

`ISA`  
- ISA는 명령어 집합 구조를 의미하는데, 프로세서의 구조와 작동 방식을 결정하는 중요한 요소이다.  

- 하드웨어와 소프트웨어 사이의 상호 작용의 기준을 제공한다.

1. 즉 소프트웨어는 ISA의 기준을 바탕으로 설계 제작 및 실행
2. 소프트웨어 코드가 컴파일러를 통해 해당 ISA의 기준에 맞게 기계어로 변환
3. 해석된 기계어(명령어)를 하드웨어가 ISA의 기준으로 명령어를 해석하고 수행
<br/>
<br/>


> #### 6. 참고 문헌
[ChatGPT](https://openai.com/blog/chatgpt)
[인용세상](https://inyongs.tistory.com/108)
[세아향](https://news.skhynix.co.kr/post/mobile-not-cpu)

