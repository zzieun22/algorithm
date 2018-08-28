## 1. SSD 의 구조 

### 1.1. NAND 플래시 메모리 셀

SSD 는 플래시 메모리를 기반으로한 저장 매체이다. 비트들은 Floating-Gate 트랜지스터로 구성된 셀에 저장된다. SSD는 모든 컴포넌트가 전기장치이며 , HDD와 같은 기계장치를 가지고 있지 않다. 

 Floating-Gate 트랜지스터에 전압이 가해지면서 셀의 비트가 쓰여지거나 읽혀지게 된다. 트랜지스터는 NOR 플래시 메모리와 NAND플래시 메모리 두 종류가 있는데 NAND에 대해서 살펴보도록 한다. 

NAND플래시 메모리의 모듈의 중요한 속성은 수명이 제한적이라는 것이다. 트랜지스터는 셀에 전자를 저장하면서 쓰기를 하게 되는데 , 매번 P/E(program & Erase 쓰고 &지우기) 사이클마다 일부 전자가 오류로 인해서 트랜지스터에 갇히게 된다. 그리고 이렇게 갇힌 전자들이 쌓여서 일정수준이상 넘어서게 되면 , 그 셀은 불가능한 상태가 되는 것이다. 



> 수명제한 
>
> 각 셀은 최대 P/E 사이클을 가지는데, 이 사이클을 넘어서면 결함 셀로 간주된다. NAND플래시 메모리는 수명제한을 가지는데, 이 수명 제한은 NAND 플래시 메모리의 타입에 따라서 조금 씩 차이가 있다. 



최근 연구에 따르면, NAND 플래시 메모리 칩에 높은 열이 가해지면 각 셀에 갇혀있던(프로그램되어 있던) 전자들이 사라진다는 것이 확인되었다. 또한 아직 연구중이며 언제 시중에 이런 제품이 출시될지 모르지만, SSD의 수명을 상당히 높일 수 있는 방법도 있다. 현재 시중에 출시되는 SSD의 메모리 셀 타입은:

- SLC (Single level cell), SLC 트랜지스터는 단 하나의 비트만 저장할 수 있지만 상대적으로 긴 수명을 가짐
- MLC (Multiple level cell), MLC 트랜지스터는 2 비트를 저장할 수 있으며, SLC에 비해서 상대적으로 레이턴시(응답 속도)가 높고 짧은 수명을 가짐
- TLC (Triple-level cell), TLC 트랜지스터는 3 비트를 저장할 수 있지만, MLC보다 레이턴시가 높고 수명이 더 짧음

> ##### 메모리 셀 타입
>
> SSD는 플래시 메모리 기반의 저장 매체이다. 비트는 셀에 저장되며, 메모리 셀은 3가지 타입이 있다. SLC는 1비트, MLC는 2비트 그리고 TLC는 3비트를 저장할 수 있다.

표1은 각 NAND 플래시 셀 타입의 상세한 정보를 보여주고 있다. 비교를 위해서 HDD와 메인 메모리(RAM) 그리고 L1/L2 캐시도 같이 포함하였다.

|                    | SLC  | MLC  | TLC  | HDD       | RAM      | L1 cache | L2 cache |
| ------------------ | ---- | ---- | ---- | --------- | -------- | -------- | -------- |
| P/E cycles         | 100k | 10k  | 5k   | *         | *        | *        | *        |
| Bits per cell      | 1    | 2    | 3    | *         | *        | *        | *        |
| Seek latency (μs)  | *    | *    | *    | 9000      | *        | *        | *        |
| Read latency (μs)  | 25   | 50   | 100  | 2000-7000 | 0.04-0.1 | 0.001    | 0.004    |
| Write latency (μs) | 250  | 900  | 1500 | 2000-7000 | 0.04-0.1 | 0.001    | 0.004    |
| Erase latency (μs) | 1500 | 3000 | 5000 | *         | *        | *        | *        |

SLC 타입의 SSD는 MLC타입보다 신뢰성이 높고 더 오랜 시간 사용할 수 있지만, 제조비용이 크다. 그래서 대 부분의 SSD는 MLC또는 TCL타입이며, 엔터프라이즈급의 SSD만 SLC를 사용한다. 워크로드에 맞게 적절한 메모리타입을 선정하는 것이 중요한데, 여기서 워크로드라 함은 얼마자 SSD에 데이터가 자주 기록되는지를 의미한다. 예를 들어서 쓰기 빈도가 아주 높다면 SLC 타입의 SSD가 최선이며, (동영상 스트리밍을 위한 저장소와 같이) 쓰기는 많지 않지만 읽기가 매우 많다면 TLC가 가장 적절한 선택이 될 것이다. 게다가 실제 서비스 수준의 워크로드로 수행된 벤치마크의 결과에 의하면, 실제 TLC 타입의 메모리 수명은 크게 문제되지 않는 것으로 보인다



> NAND플래시 페이지와 블록
>
> 셀들은 Block으로 그룹핑 되어있으며, Block들은 다시 Plane으로 그룹핑 되어 있다. SSD를 읽고 쓰기시에 최소 접근 단위는 Page라고 하며, Page는 개별로 삭제될 수 없고 반드시 삭제는 Block단위로만 수행될 수 있다. NAND플래시 메모리의 Pagezmrlsms 2,4,8,16KB 로 다양한데, 대부분 SSD의 Block는 128개 또는 256개의 Page를 가진다. 이는 SSD 제조사별로 Block의 크기가 256KB에서 4KB까지 다양하다는 것을 의미한다. 를 들어서 삼성SSD 840 EVOsms 2048KB의 Block크기를 가지며 각Block는 256개의 8KB페이지를 가진다.



### 1.2 SSD의 구성



 ![스크린샷 2018-08-28 오후 1.48.37](/Users/PARKHASIK/Desktop/스크린샷 2018-08-28 오후 1.48.37.png)



사용자의 요청은 호스트 인터페이스를 통해서 유입되는데, 가장 일반적인 인터페이스 방식은 ATA(SATA) , PCL Express(PCle)타입이다. SSD컨트롤러에 장착된 프로세서가 명령을 받아서 컨트롤러로 전달하게 된다. SSD는 자체적으로 보드에 저장된 내장 메모리(RAM)을 가지는데, 일반적으로 이 메모리는 맵핑 정보를 저장하거나 캐시용도로 사용된다.

 



## SSD의 핵심기술 FTL 

#### 1. FTL이란?

플래시 메모리와 파일시스템 사이에 위치하면서, 플래시 메모리를 디스크 처럼 사용할 수 있게 해주는 사상(Mapping) 기술 

**FTL필요성**

업계에 SSD가 이렇게 쉽게 받아들여지게 만든 중요한 요소 중 하나는 SSD가 HDD와 동일한 호스트 인터페이스를 이용한다는 것이다. 물론 현재의 LBA(Logical Block Address) 어레이는 덮어쓰기가 가능한 HDD에서만 적합한 요소이며, SSD에는 적합하진 않지만 말이다. 이 때문에 NAND 플래시 메모리는 내부적인 특성을 숨기고 LBA 어레이를 호스트로 노출하기 위해서 부가적인 컴포넌트를 필요로 한다. 이 컴포넌트를 FTL(Flash Translation Layer)하고 하며, SSD 컨터롤러 내부에 위치하고 있다. FTL은 아주 중요한 역할을 담당하며, 논리적 블록 맵핑(Logical Block Mapping)과 Garbage-collection 2개의 중요한 부분을 담당한다.

- 플래시 메모리 섹터들의 최대 지우기 횟수의 유한한 결점을 보완하는 역할 
- DISK I/O의 플래시 메모리에서 동작 할 수 있도록 지원
- NAND 플래시 한계 : 쓰기보다 지우기가 느린 특성 



#### 2. FTL계층 구조 및 구성요소 , 핵심기술  

![스크린샷 2018-08-25 오후 4.08.09](/Users/PARKHASIK/Desktop/스크린샷 2018-08-25 오후 4.08.09.png)



**FTL 의 구성요소**

1. STL(Sector Translation Layer)

   - Address Mapping, Garbage Collection, Wear Leveling 등 담당
   - Adress Mapping : 파일 시스템으로부터 논리적 주소를 NAND Flash Memory의 물리적 주소로 연결

2.  BML(Bad-Block Management Layer)

   - 무효화된 페이지를 많이 가지고 있는 블록을 선택후, 유효 페이지를 다른 블록에 복사하여 블록을 삭제하여 배 사용해주는 기법

3. LLD(Low Level Driver)

   - NAND Flash 를 사용하기 위한 Driver

   

**FTL의 핵심기술**

1. Wear Leveling

- 블록당 Writer 횟수를 모니터 하여, 균등하게 분배
- 특정 블록에만 Write 되는 것을 방지하여, 수명연장

2. Garbage Collection

- 블록을 실제로 삭제하지 않고, 표기 후 적절한 시점에 일괄 삭제 처리하는 기술
- Java의 GC와 유사한 기술, 삭제 효율화

3. Over Provisioning

- Wear Leveling, GC를 작업하기 위한 여유공간

  

**3. FTL의 매핑방식**

1. Secector Mapping

- read, write 단위인 섹터 단위로 사상테이블 생성
- 장점 : upate 시, 정보만 변경한 data를 쓸 수 있음. erase 연산 최소화
- 단점 : 사상테이블 크기가 커질 수 있음

2. Block Mapping

- Erase 단위인 블록 단위로 사상테이블 생성
- 장점 : 사상테이블의 크기 최소화 가능
- 단점 : 같은 논리 주소에 쓰기 연산이 많이 요청되는 경우 성능저하

3. Hybrid Mapping

- Sector와 Block을 혼합한 기법
- 장점 : Sector와 Block의 장점 혼합
- 단점 : 구현의 복잡한 문제 존재



## FTL : Flash Translation Layer

플래시 메모리는 비 휘발성(non-volatility), 빠른 접근 속도, 저전력 소비, 그리고 간편한 휴대성 등의 장점을 가지므로 최근에 많은 임베디드 시스템에서 많이 사용되고 있다.
그런데 플래시 메모리는 그 하드웨어 특성상 플래시 변환 계층(FTL: flash translation layer)이라는 시스템 소프트웨어를 필요로한다

> FTL(Flash Translation Layer)은 호스트의 LBA(Logical Block Address)와 드라이브의 PBA(Physical Block Address)를 맵핑해주는 SSD 컨트롤러의 컴포넌트이다. 가장 최근의 드라이브는 Log Structure 파일 시스템과 같이 작동하는 “hybrid log-block mapping” 또는 그 파생 알고리즘을 구현하고 있다. 이 알고리즘은 랜덤 쓰기를 시퀀셜 쓰기처럼 핸들링할 수 있도록 해준다.


FTL의 주요 기능은 파일 시스템으로부터 내려오는 논리 주소를 플래시 메모리의 물리 주소로 변환하는 일이다.



기존의 메모리와는 달리 반도체 기반의 플래시 메모리는 몇가지 성질을 가지고 있습니

1.  **쓰기 전 지우기** : 기존의 비 휘발성 메모리인 하드 디스크는 데이타 갱신의 경우에도 바로 데이타 가 갱신될 수 있지만 플래시 메모리는 데이타 갱신 연산을 수행하기 위해서는 반드시 그 데이타를 포함 한 영역이 지워져 있어야 하는 성질을 가진다.


2. **배드 섹터의 존재** : 플래시 메모리는 출하할 당시 또는 데이타 쓰기 연산을 하는 중에 해당 섹터가 배드가 될 수 있는 성질을 가진다.


3. **마모도 평준화(wear-leveling) 요구** : 플래시 메모리는 특정 섹터에 대한 쓰기 연산이 일정 횟수를 넘으면 그 섹터의 데이타 정보가 손상될 수 있는 특징을 가진다.

플래시 메모리 기반의 시스템은 이와 같은 플래시 메모리의 본질적인 하드웨어적인 한계점을 극복하기 위하여 시스템 소프트웨어가 필요한데 이 소프트웨어를 **플래시 사상 단계(FTL: Flash Translation Layer)**라고 한다고 합니다.

특히 플래시 메모리의 첫 번째 특성인 "쓰기 전 지우기" 성질은 일반 플래시 메모리의 지우기 단위와 쓰기 단위가 다르기 때문에 플래시 기반 임베디드 시스템의 성능 저하의 주요한 원인이 되고 있습니다.

기존의  FTL 알고리즘에서 이러한 문제를 극복하기 위해서 주로 사용한 방법이 **논리 물리 사상** 방법입니다.

**논리 물리 사상 테이블을 이용하여 해당 물리 주소가 이미 데이터가 쓰여져 있으면 비어있는 플래시 메모리의 공간에 먼저 쓴 후 논리 물리 사상 테이블을 변경하는 방법입니다.**



그러나 이 FTL 알고리즘을 실제 임베디드 시스템에 적용 할때 문제점이 사상 정보를 위한 메모리용량 입니다.

사상 정보는 성능을 위하여 값 비싼 RAM에 저장되는데 사상 정보의 양이 많아지면 RAM 사용량 또한 증가 되고 전체 비용 면에서 FTL 알고리즘이 실제 임베디드 시스템에서 적용되기 힘들게 됩니다.

[카카오 기술 블로그 FTL](http://tech.kakao.com/2016/07/15/coding-for-ssd-part-3/)
