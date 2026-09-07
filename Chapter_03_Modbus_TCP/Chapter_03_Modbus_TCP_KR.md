**Volume 07. Industrial Communication**

# Chapter 03. Modbus TCP

## 03.01. Modbus TCP Architecture

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

모드버스 TCP(Modbus TCP)는 모드버스 응용 프로토콜(Modbus Application Protocol)을 표준 이더넷(Ethernet) 및 TCP/IP 네트워크로 확장하여 산업용 제어기(Industrial Controller), 로봇(Robot), 센서(Sensor), 드라이브(Drive), 상위 감시 시스템(Supervisory System)이 널리 사용되는 네트워크 인프라를 통해 구조화된 공정 데이터(Process Data)를 교환할 수 있도록 한다. 전용 직렬 멀티드롭 버스(Serial Multidrop Bus)에 의존하는 대신 각 장치는 이더넷 인터페이스(Ethernet Interface)와 IP 주소(IP Address)를 통해 통신한다.

프로토콜 아키텍처(Protocol Architecture)는 모드버스 응용 프로토콜(Modbus Application Protocol)과 TCP/IP 통신 스택(Communication Stack)이 계층적으로 결합된 구조로 이해할 수 있다. 모드버스(Modbus)는 요청(Request), 응답(Response), 기능 코드(Function Code), 레지스터 주소(Register Address), 데이터 값(Data Value)의 의미를 정의하며, TCP는 네트워크 종단점(Network Endpoint) 사이의 신뢰성 있는 전송을 담당한다. IP는 주소 지정과 라우팅(Routing)을 처리하고 이더넷(Ethernet)은 프레임(Frame)을 전달한다.

일반적인 모드버스 TCP(Modbus TCP) 시스템은 클라이언트-서버 통신 모델(Client-Server Communication Model)을 따른다. 클라이언트(Client)는 홀딩 레지스터(Holding Register)를 읽거나 출력 값을 기록하는 것과 같은 원하는 동작을 지정하여 트랜잭션(Transaction)을 시작한다. 서버(Server)는 요청을 수신하고 지정된 기능을 처리한 후 내부 모드버스 데이터 모델(Modbus Data Model)에 접근하여 응답한다. PLC, 산업용 PC(Industrial PC), 엣지 컴퓨터(Edge Computer), 로봇 제어기(Robot Controller) 등이 일반적으로 클라이언트 역할을 수행한다.

통신은 일반적으로 TCP 포트 502(TCP Port 502)를 사용하며, 각각의 모드버스 TCP 연결(Modbus TCP Connection)은 두 IP 종단점(IP Endpoint) 사이의 표준 TCP 세션(TCP Session)으로 구성된다. TCP는 응용 데이터를 교환하기 전에 연결을 설정하기 때문에 모드버스 메시지(Modbus Message)는 전송 계층(Transport Layer)이 제공하는 순서 제어(Sequencing), 재전송(Retransmission), 오류 검출(Error Detection), 흐름 제어(Flow Control)의 이점을 활용할 수 있다.

모드버스 TCP 응용 데이터 단위(Modbus TCP Application Data Unit)는 MBAP 헤더(MBAP Header)와 그 뒤에 위치하는 프로토콜 데이터 단위(Protocol Data Unit)로 구성된다. MBAP는 모드버스 응용 프로토콜(Modbus Application Protocol)을 의미하며 TCP/IP에서 트랜잭션을 관리하는 데 필요한 정보를 제공한다. 주요 필드는 트랜잭션 식별자(Transaction Identifier), 프로토콜 식별자(Protocol Identifier), 길이(Length), 유닛 식별자(Unit Identifier)이며, 이후 기능 코드(Function Code)와 관련 데이터가 전달된다.

트랜잭션 식별자(Transaction Identifier)는 이더넷 통신(Ethernet Communication)에서 특히 중요하다. 하나의 클라이언트(Client)가 여러 요청을 전송할 수 있기 때문에 수신한 각각의 응답을 올바른 요청과 연결해야 하기 때문이다. 서버(Server)는 요청에 포함된 트랜잭션 식별자를 응답에 그대로 복사한다. 프로토콜 식별자(Protocol Identifier)는 일반적인 모드버스에서 0으로 설정되며, 길이 필드(Length Field)는 이후에 이어지는 메시지의 크기를 나타낸다.

모드버스 RTU(Modbus RTU)와 달리 모드버스 TCP(Modbus TCP)는 각 응용 프레임(Application Frame)의 끝에 모드버스 CRC(Modbus CRC)를 추가할 필요가 없다. 이더넷(Ethernet)과 TCP가 이미 전송 오류를 검출하기 위한 메커니즘을 제공하기 때문에 직렬 모드버스(Serial Modbus)의 CRC 기능을 반복할 필요가 없다. 이는 모드버스 응용 의미(Modbus Application Semantics)는 대부분 유지하면서 전송 방식에 따른 프레이밍 메커니즘(Framing Mechanism)을 하위 네트워크 특성에 맞게 변경하는 구조적 특징을 보여준다.

유닛 식별자(Unit Identifier)는 이더넷(Ethernet)과 기존 모드버스 직렬 네트워크(Modbus Serial Network)를 연결하는 구조적 브리지(Bridge) 역할을 한다. 네이티브 모드버스 TCP 서버(Native Modbus TCP Server)와 직접 통신할 때는 목적지가 IP 주소로 이미 식별되므로 중요성이 상대적으로 낮다. 그러나 모드버스 TCP-RTU 게이트웨이(Modbus TCP-to-RTU Gateway)를 사용하는 경우 유닛 식별자를 이용해 하위 직렬 장치를 지정할 수 있어 하나의 이더넷 게이트웨이(Ethernet Gateway)가 여러 RS485 장치에 대한 접근을 제공할 수 있다.

응용 계층(Application Layer)에서 모드버스(Modbus)는 정보를 전통적으로 코일(Coil), 디스크리트 입력(Discrete Input), 입력 레지스터(Input Register), 홀딩 레지스터(Holding Register)라는 논리적 데이터 영역으로 구성한다. 코일과 디스크리트 입력은 단일 비트(Bit) 정보를 나타내며 입력 및 홀딩 레지스터는 16비트 값을 나타낸다. 각 주소의 실제 공학적 의미는 장치의 레지스터 맵(Register Map)에 의해 결정되며 모터 속도, 전류, 온도, 경보 상태, 센서 측정값, 운전 명령 또는 설정 파라미터와 연결될 수 있다.

이더넷 스위칭(Ethernet Switching)은 RS485 멀티드롭 배선(RS485 Multidrop Wiring)과 비교하여 물리적 통신 아키텍처(Physical Communication Architecture)를 근본적으로 변화시킨다. 장치는 산업용 이더넷 스위치(Industrial Ethernet Switch)를 통해 스타(Star), 트리(Tree), 계층형 네트워크(Hierarchical Network) 구조로 연결될 수 있으며 라우팅이 허용되는 경우 여러 네트워크 세그먼트(Network Segment)에 걸쳐 통신할 수 있다. 이러한 확장성은 PLC, AMR, 검사 시스템, 게이트웨이, HMI 및 상위 컴퓨터가 함께 동작하는 산업 환경에서 유용하다.

모드버스 TCP 클라이언트(Modbus TCP Client)는 여러 서버(Server)에 대한 연결을 유지하면서 응용 요구사항에 따라 서로 다른 레지스터 영역(Register Range)을 주기적으로 폴링(Polling)할 수 있다. 빠르게 변화하는 공정 변수(Process Variable)는 높은 빈도로 읽을 수 있고 설정 값이나 진단 정보(Diagnostic Information)는 상대적으로 낮은 빈도로 요청할 수 있다. 모드버스는 기본적으로 요청-응답(Request-Response) 방식이므로 네트워크 부하와 갱신 지연(Update Latency)은 폴링 주기, 트랜잭션 크기, 장치 응답 시간, 연결 관리 및 서버 수에 크게 영향을 받는다.

로보틱스(Robotics)와 자율이동로봇(AMR) 시스템에서 모드버스 TCP(Modbus TCP)는 높은 결정론적 모션 제어(Deterministic Motion Control)가 필요하지 않은 산업 장비를 통합하는 데 특히 유용하다. AMR의 엣지 컴퓨터(Edge Computer)나 PLC는 이더넷을 통해 배터리 충전기(Battery Charger), 전력계(Power Meter), 원격 입출력(Remote I/O), 환경 센서(Environmental Sensor), 컨베이어(Conveyor), 자동문, 리프트(Lift), 공장 설비와 상태 및 명령 데이터를 교환할 수 있다. 따라서 주로 고속 모션 네트워크보다는 장비 통합 인터페이스(Equipment Integration Interface)로 활용된다.

실제 로봇 아키텍처(Robot Architecture)에서는 동일한 이더넷 기술을 사용하더라도 모드버스 TCP(Modbus TCP) 트래픽을 다른 통신 도메인(Communication Domain)과 분리할 수 있다. 실시간 모션 네트워크(Real-Time Motion Network), 카메라 스트림(Camera Stream), LiDAR 데이터, ROS 2 통신, 플릿 통신(Fleet Communication), 진단 및 모드버스 장비 인터페이스는 서로 다른 대역폭과 시간 특성을 가진다. 관리형 스위치(Managed Switch), VLAN, 라우팅 정책(Routing Policy), 네트워크 세분화(Network Segmentation)를 적용하면 우선순위가 낮은 폴링 트래픽이 중요한 통신 경로에 영향을 주는 것을 방지할 수 있다.

모드버스 TCP(Modbus TCP)의 단순성은 중요한 공학적 장점 중 하나이다. 장치가 네트워크에 참여하기 위해 복잡한 객체 모델(Object Model)을 제공할 필요 없이 명확하게 정의된 레지스터 맵(Register Map)을 통해 운전 변수를 제공하고 필요한 기능 코드(Function Code)를 구현하면 된다. 따라서 서로 다른 제조사의 장비도 비교적 쉽게 통합할 수 있다. 그러나 실제 상호운용성(Interoperability)을 확보하려면 레지스터 주소, 데이터 형식, 바이트 및 워드 순서(Byte and Word Ordering), 스케일링 계수(Scaling Factor), 접근 권한, 예외 처리(Exception Handling), 갱신 동작을 정확히 정의해야 한다.

시스템 아키텍처(System Architecture)의 관점에서 모드버스 TCP(Modbus TCP)는 단순히 직렬 프로토콜(Serial Protocol)을 이더넷으로 운반하는 기술이 아니라 계층화된 산업 통신 메커니즘(Layered Industrial Communication Mechanism)으로 이해해야 한다. 이더넷(Ethernet)은 물리적 연결성을 제공하고, IP는 네트워크 주소 지정(Network Addressing)을 담당하며, TCP는 신뢰성 있는 전송을 제공한다. MBAP 헤더(MBAP Header)는 트랜잭션을 관리하고 모드버스 PDU(Modbus PDU)는 산업용 동작과 데이터 접근을 정의함으로써 기존 모드버스 레지스터 모델을 현대적인 PLC, 공장 자동화(Factory Automation), 로보틱스 및 AMR 이더넷 네트워크에서 활용할 수 있도록 한다.

## 03.02. Function Code Reference

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

모드버스 기능 코드(Modbus Function Code)는 클라이언트(Client)가 서버(Server)에 요청하는 동작을 정의하며, 모드버스 응용 프로토콜(Modbus Application Protocol)의 명령 체계를 구성한다. 각 요청(Request)에는 기능 코드(Function Code)와 함께 시작 주소(Starting Address), 데이터 수량(Quantity), 값(Value) 등 해당 동작에 필요한 데이터가 포함된다. 서버는 기능 코드를 해석하고 해당 데이터 영역에 접근하여 요청된 동작을 수행한 뒤, 정상적으로 처리된 경우 동일한 기능 코드가 포함된 응답(Response)을 반환한다.

기능 코드(Function Code)는 코일(Coils), 디스크리트 입력(Discrete Inputs), 입력 레지스터(Input Registers), 홀딩 레지스터(Holding Registers)라는 네 가지 전통적인 모드버스 데이터 영역(Modbus Data Area)과 밀접하게 관련되어 있다. 코일은 단일 비트(Bit)의 읽기/쓰기 값을 나타내고, 디스크리트 입력은 단일 비트의 읽기 전용 상태를 나타낸다. 입력 레지스터는 일반적으로 16비트 읽기 전용 정보를 저장하며, 홀딩 레지스터는 일반적으로 읽기와 쓰기가 가능한 16비트 값을 제공한다.

기능 코드 01(Function Code 01)인 코일 읽기(Read Coils)는 클라이언트가 서버에 있는 여러 코일의 켜짐(ON) 또는 꺼짐(OFF) 상태를 읽을 수 있도록 한다. 요청은 시작 코일 주소(Starting Coil Address)와 읽을 코일의 개수를 지정한다. 응답에서는 여러 불리언 상태(Boolean State)를 바이트(Byte) 단위로 패킹(Packing)하며, 각각의 비트가 하나의 코일 상태를 나타낸다. 이 기능은 디지털 출력, 명령 상태, 활성화 신호, 릴레이 상태와 같은 이진 변수를 확인하는 데 주로 사용된다.

기능 코드 02(Function Code 02)인 디스크리트 입력 읽기(Read Discrete Inputs)는 하나 이상의 디스크리트 입력 상태를 읽는다. 요청 및 응답 구조는 코일 읽기(Read Coils)와 유사하지만, 대상 데이터는 일반적으로 모드버스 클라이언트 관점에서 읽기 전용(Read-Only)이다. 대표적인 예로 리미트 스위치(Limit Switch), 근접 센서(Proximity Sensor), 접점 입력(Contact Input), 안전 관련 상태 표시 및 장비 준비 신호(Equipment-Ready Signal)가 있으며, 여러 입력 상태를 하나의 응답에 효율적으로 패킹할 수 있다.

기능 코드 03(Function Code 03)인 홀딩 레지스터 읽기(Read Holding Registers)는 가장 널리 사용되는 모드버스 동작 중 하나이다. 지정된 주소부터 시작하여 하나 이상의 연속된 16비트 홀딩 레지스터를 읽는다. 홀딩 레지스터에는 설정 파라미터(Configuration Parameter), 명령 값(Command Value), 측정값, 카운터(Counter), 운전 모드(Operating Mode), 내부 장비 상태 등이 저장될 수 있다. 32비트 정수나 부동소수점(Floating-Point) 값은 여러 레지스터를 사용하므로 레지스터 순서, 바이트 순서, 스케일링(Scaling), 부호 처리에 대한 합의가 필요하다.

기능 코드 04(Function Code 04)인 입력 레지스터 읽기(Read Input Registers)는 입력 레지스터 데이터 영역에서 16비트 값을 읽는다. 이러한 레지스터는 일반적으로 읽기 전용 공정 정보(Read-Only Process Information)로 취급되며 장치가 생성한 측정값이나 계산값을 나타낸다. 온도, 전압, 전류, 압력, 속도, 위치, 에너지 소비량 및 센서 측정값 등이 입력 레지스터를 통해 제공될 수 있다. 정확한 데이터 의미는 장치별로 다르므로 제조사의 레지스터 맵(Register Map)을 통해 정의해야 한다.

기능 코드 05(Function Code 05)인 단일 코일 쓰기(Write Single Coil)는 하나의 개별 코일을 켜짐(ON) 또는 꺼짐(OFF) 상태로 설정한다. 클라이언트가 활성화 명령(Enable Command), 리셋 요청(Reset Request), 릴레이 출력, 시작 신호(Start Signal), 액추에이터 상태(Actuator State)와 같은 단일 불리언 변수를 제어할 때 유용하다. 서버는 요청된 주소와 값을 검증하고 허용된 경우 해당 코일을 갱신한 뒤, 일반적으로 성공적인 처리를 나타내기 위해 요청에 포함된 관련 정보를 응답으로 반환한다.

기능 코드 06(Function Code 06)인 단일 레지스터 쓰기(Write Single Register)는 하나의 홀딩 레지스터에 단일 16비트 값을 기록한다. 이 기능은 하나의 레지스터에 저장할 수 있는 단순한 설정 파라미터, 운전 모드, 설정값(Setpoint), 명령 값 또는 제어 변수(Control Variable)를 변경할 때 주로 사용된다. 프로토콜 자체는 16비트 원시 값(Raw Value)을 전달하지만 실제 공학적 의미는 레지스터 정의에 따라 달라지며, 예를 들어 정수 값에 지정된 스케일 계수(Scale Factor)를 적용하여 속도를 표현할 수 있다.

기능 코드 15(Function Code 15)인 다중 코일 쓰기(Write Multiple Coils)는 하나의 트랜잭션(Transaction)을 통해 연속된 여러 코일의 상태를 갱신할 수 있도록 한다. 요청에는 시작 주소, 코일 개수, 바이트 수(Byte Count), 패킹된 출력 값(Packed Output Value)이 포함된다. 여러 관련 불리언 명령을 동시에 변경해야 할 경우 단일 코일 쓰기를 반복하는 것보다 효율적이다. 산업용 제어기는 이를 이용하여 출력 명령, 운전 선택, 인터록(Interlock), 장비 제어 플래그(Control Flag) 등을 그룹 단위로 전달할 수 있다.

기능 코드 16(Function Code 16)인 다중 레지스터 쓰기(Write Multiple Registers)는 연속된 여러 홀딩 레지스터에 하나의 요청으로 값을 기록하며 구조화된 산업 데이터(Structured Industrial Data)를 처리하는 데 특히 중요하다. 여러 파라미터, 설정값, 명령 블록(Command Block), 또는 하나 이상의 16비트 레지스터를 사용하는 값을 전송할 수 있다. 32비트 정수, 부동소수점 값 또는 더 큰 응용 데이터 구조가 여러 레지스터에 걸쳐 저장될 수 있으므로 레지스터 순서와 바이트 및 워드 순서(Byte and Word Ordering)를 일관되게 정의해야 한다.

기능 코드 22(Function Code 22)인 마스크 레지스터 쓰기(Mask Write Register)는 홀딩 레지스터 전체의 비트를 명시적으로 교체하지 않고 선택된 비트만 수정할 수 있도록 한다. 이 동작은 모드버스에서 정의된 처리 규칙에 따라 현재 레지스터 값에 AND 마스크(AND Mask)와 OR 마스크(OR Mask)를 적용한다. 하나의 홀딩 레지스터 안에 여러 독립적인 플래그(Flag)나 제어 필드(Control Field)가 포함되어 있을 때 나머지 정보를 유지하면서 특정 비트만 변경해야 하는 경우 유용하다.

기능 코드 23(Function Code 23)인 다중 레지스터 읽기/쓰기(Read/Write Multiple Registers)는 하나의 모드버스 트랜잭션 안에서 홀딩 레지스터 읽기와 쓰기를 결합한다. 제어기가 출력 정보를 갱신하면서 동시에 관련 공정 데이터를 가져와야 하는 경우 통신 오버헤드(Communication Overhead)를 줄일 수 있다. 서버는 정의된 쓰기 및 읽기 동작을 수행하고 요청된 레지스터 내용을 반환하며, 산업용 제어기와 장비 사이에서 밀접하게 연관된 명령 및 상태(Command-and-Status) 정보를 교환하는 데 활용할 수 있다.

모드버스(Modbus)는 일반적인 공정 데이터 동작 외에도 진단(Diagnostic) 및 장치 정보(Device Information)와 관련된 기능을 정의한다. 기능 코드 08(Function Code 08)은 주로 직렬 모드버스(Serial Modbus) 구현과 관련된 진단 기능을 제공하며, 기능 코드 43(Function Code 43)은 캡슐화 인터페이스 전송(Encapsulated Interface Transport)을 지원하고 장치 식별 정보 읽기(Read Device Identification)와 같은 기능에 사용할 수 있다. 실제 지원 범위는 제품마다 크게 다르므로 응용 시스템에서 필요한 기능과 특정 장치가 선택적으로 구현한 기능을 구분해야 한다.

서버가 정상적으로 수신한 요청을 실행할 수 없는 경우에는 정상 응답 대신 예외 응답(Exception Response)을 반환한다. 반환되는 기능 코드의 최상위 비트(Most Significant Bit)가 설정되며, 예외 코드(Exception Code)를 통해 실패 원인을 나타낸다. 대표적인 상태에는 지원되지 않는 기능(Illegal Function), 잘못된 데이터 주소(Illegal Data Address), 잘못된 데이터 값(Illegal Data Value), 서버 장치 오류(Server Device Failure)가 있다. 클라이언트 소프트웨어는 모든 수신 프레임을 정상 데이터로 해석하지 않고 이러한 예외 응답을 명시적으로 처리해야 한다.

지원되지 않는 기능(Illegal Function)은 요청된 기능이 서버에서 지원되지 않는다는 것을 의미하며, 잘못된 데이터 주소(Illegal Data Address)는 요청한 코일이나 레지스터 영역을 사용할 수 없다는 것을 의미한다. 잘못된 데이터 값(Illegal Data Value)은 일반적으로 요청 파라미터가 해당 동작에서 유효하지 않음을 나타낸다. 이러한 구분은 시운전(Commissioning) 과정에서 지원되지 않는 명령, 잘못된 레지스터 매핑(Register Mapping), 비정상 요청(Malformed Request), 내부 장치 문제를 구별하는 데 유용하다.

따라서 기능 코드 선택(Function-Code Selection)은 레지스터 맵 설계(Register-Map Design)와 독립적으로 결정해서는 안 되며 서로 연계되어야 한다. 잘 설계된 장치 인터페이스(Device Interface)는 각 변수의 데이터 영역, 주소, 접근 방향, 데이터 형식(Data Type), 크기, 스케일링, 단위(Units), 유효 범위(Valid Range), 갱신 동작(Update Behavior), 지원 기능 코드를 명확하게 정의한다. 이를 통해 클라이언트는 측정값과 명령, 읽기 전용 정보와 쓰기 가능한 파라미터를 구분하고 PLC, 산업용 PC, 로봇, 센서, 드라이브 및 게이트웨이를 보다 명확하게 통합할 수 있다.

모드버스 TCP(Modbus TCP) 시스템에서 이러한 기능 코드는 프로토콜 데이터 단위(Protocol Data Unit, PDU) 내부에 전달되며, MBAP 헤더(MBAP Header)와 TCP/IP 계층은 트랜잭션 식별(Transaction Identification)과 네트워크 전송(Network Transport)을 관리한다. 기능 코드의 의미는 이더넷 연결 여부와 관계없이 기본적으로 모드버스 체계를 유지한다. 이러한 계층 분리를 통해 레지스터 읽기와 코일 쓰기 같은 기존 동작을 현대적인 산업용 이더넷 네트워크에서 사용하면서도 게이트웨이 및 광범위한 모드버스 데이터 모델과의 호환성을 유지할 수 있다.

## 03.03. Register Map Design

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

모드버스 레지스터 맵(Modbus Register Map)은 응용 데이터(Application Data)가 모드버스 프로토콜(Modbus Protocol)을 통해 어떻게 구성되고, 주소가 지정되며, 해석되고, 접근되는지를 정의한다. 모드버스는 코일(Coils), 디스크리트 입력(Discrete Inputs), 입력 레지스터(Input Registers), 홀딩 레지스터(Holding Registers)와 같은 표준 논리 영역을 제공하지만 개별 주소의 공학적 의미까지 정의하지는 않는다. 따라서 레지스터 맵 설계(Register-Map Design)는 장치 구현과 PLC, 산업용 PC, 로봇 제어기, 게이트웨이 및 상위 시스템 사이의 인터페이스 계약 역할을 한다.

잘 설계된 레지스터 맵(Register Map)은 먼저 정보의 목적과 접근 특성에 따라 데이터를 분리하는 것에서 시작한다. 쓰기가 필요한 이진 명령(Binary Command)은 코일(Coils)로 표현할 수 있으며, 이진 상태 정보(Binary Status Information)는 디스크리트 입력(Discrete Inputs)을 사용할 수 있다. 측정되거나 계산된 읽기 전용 값은 입력 레지스터(Input Registers)에 배치하고, 설정 가능한 파라미터나 명령 값은 홀딩 레지스터(Holding Registers)에 배치할 수 있다. 이러한 논리 영역을 일관되게 사용하면 인터페이스를 이해하기 쉬워지고 상태 또는 측정 데이터에 대한 의도하지 않은 쓰기를 줄일 수 있다.

주소 할당(Address Allocation)은 새로운 변수가 추가될 때마다 개별적으로 레지스터를 배정하는 방식이 아니라 계획된 구조를 따라야 한다. 관련 정보는 장치 식별(Device Identification), 운전 상태(Operating Status), 명령(Commands), 센서 측정값(Sensor Measurements), 설정 파라미터(Configuration Parameters), 진단(Diagnostics), 경보(Alarms), 유지보수 정보(Maintenance Information), 통신 설정(Communication Settings) 등의 기능 블록으로 그룹화할 수 있다. 주요 블록 사이에 사용하지 않는 주소를 예약하면 기존 주소 체계를 변경하지 않고 향후 기능을 확장할 수 있다.

모드버스 문서(Modbus Documentation)에서는 일반적으로 코일에 0xxxx, 디스크리트 입력에 1xxxx, 입력 레지스터에 3xxxx, 홀딩 레지스터에 4xxxx와 같은 참조 주소(Reference Address)를 사용한다. 이러한 참조 주소를 모드버스 프로토콜 데이터 단위(Modbus Protocol Data Unit)에 실제로 전송되는 주소와 동일하게 해석해서는 안 된다. 구현에서는 흔히 0 기반 프로토콜 오프셋(Zero-Based Protocol Offset)을 사용하지만 매뉴얼에서는 1 기반 레지스터 번호(One-Based Register Number)를 표시할 수 있으므로 주소 지정 규칙(Addressing Convention)을 명확히 정의해야 한다.

각 레지스터 항목(Register Entry)은 단순한 주소보다 훨씬 많은 정보를 설명해야 한다. 유용한 레지스터 정의에는 변수 이름(Variable Name), 논리 데이터 영역(Logical Data Area), 프로토콜 주소(Protocol Address), 접근 권한(Access Permission), 데이터 형식(Data Type), 공학 단위(Engineering Unit), 스케일링 규칙(Scaling Rule), 유효 범위(Valid Range), 기본값(Default Value), 기능 설명이 포함된다. 필요한 경우 갱신 주기(Update Rate), 영구 저장 여부(Persistence Behavior), 요구 운전 상태, 지원 기능 코드(Function Code), 쓰기 동작이 즉시 장치 동작을 실행하는지 여부도 정의해야 한다.

하나의 모드버스 레지스터(Modbus Register)는 16비트로 구성되므로 많은 응용 값(Application Value)은 하나 이상의 레지스터를 필요로 한다. 32비트 정수(Integer)는 일반적으로 연속된 두 개의 레지스터를 사용하며 64비트 값은 네 개의 레지스터를 사용한다. 부동소수점(Floating-Point), 타임스탬프(Timestamp), 카운터(Counter), 식별자(Identifier), 대형 수치 데이터도 여러 주소에 걸쳐 저장될 수 있다. 따라서 다중 레지스터 변수(Multi-Register Variable)는 연속된 블록으로 할당하고 하나의 논리 객체(Logical Object)로 문서화해야 한다.

값이 여러 레지스터에 걸쳐 저장되는 경우 바이트 순서(Byte Order)와 워드 순서(Word Order)는 매우 중요하다. 모드버스는 각각의 16비트 레지스터를 두 개의 바이트로 전송하지만, 장치 구현에 따라 32비트 또는 64비트 응용 값을 구성하는 워드의 순서가 달라질 수 있다. 따라서 레지스터 맵은 변수가 단순히 정수나 부동소수점이라고만 정의해서는 안 되며 정확한 데이터 표현 방식을 지정해야 한다. 알려진 16진수 값과 공학 값을 포함한 시험 예제(Test Example)는 시스템 통합 과정에서 올바른 데이터 해석을 확인하는 데 특히 유용하다.

스케일링(Scaling)은 부동소수점 표현을 대신하는 방법으로 산업용 레지스터 맵에서 널리 사용된다. 예를 들어 25.3도의 온도를 스케일 계수(Scale Factor) 0.1을 적용하여 정수 253으로 전송할 수 있으며, 속도 값은 0.01 RPM 단위로 표현할 수 있다. 레지스터 정의에는 원시 데이터 표현(Raw Representation)과 변환 규칙(Conversion Rule)을 모두 명시해야 하며, 이를 통해 클라이언트는 전송된 값을 실제 물리적 공학량(Engineering Quantity)으로 일관되게 변환할 수 있다.

부호 여부(Signedness) 역시 명시적으로 정의해야 한다. 하나의 16비트 레지스터는 0에서 65535까지의 부호 없는 값(Unsigned Value)을 표현하거나, 2의 보수(Two\'s Complement)를 사용하여 -32768에서 32767까지의 부호 있는 값(Signed Value)을 표현할 수 있다. 이러한 정보가 없으면 음의 온도, 토크, 오프셋(Offset), 전류, 속도, 위치 오차 등이 잘못 해석될 수 있다. 불리언 인코딩(Boolean Encoding), 열거형 운전 모드(Enumerated Operating Mode), 비트 필드(Bit Field), 무효 값(Invalid Value) 및 초기화되지 않은 측정값을 나타내는 특수 값에도 동일한 규칙을 정의해야 한다.

비트 필드 레지스터(Bit-Field Register)는 하나의 16비트 홀딩 또는 입력 레지스터 안에서 여러 불리언 상태(Boolean Condition)를 효율적으로 표현할 수 있다. 개별 비트는 준비(Ready), 운전 중(Running), 경고(Warning), 고장(Fault), 충전 중(Charging), 통신 활성(Communication Active), 비상 상태(Emergency Condition), 유지보수 필요(Maintenance Required) 등의 상태를 나타낼 수 있다. 정의된 각 비트 위치는 안정적으로 동일한 의미를 유지해야 하며, 사용하지 않는 비트는 예약(Reserved)으로 표시하여 향후 확장을 지원해야 한다.

명령 레지스터(Command Register)는 값을 기록하는 행위가 실제 물리 시스템의 동작을 변경할 수 있기 때문에 특별한 주의가 필요하다. 명령 인터페이스(Command Interface)는 영구 설정(Persistent Setting)과 시작(Start), 정지(Stop), 리셋(Reset), 승인(Acknowledge), 원점 복귀(Home), 실행(Execute)과 같은 순간 동작(Momentary Action)을 명확하게 구분해야 한다. 의도하지 않은 쓰기가 문제가 될 수 있는 경우 명시적인 명령 값, 검증 범위, 명령 식별자(Command Identifier), 명령-승인 패턴(Command-and-Acknowledgment Pattern)을 사용할 수 있다.

읽기 전용 상태 데이터(Read-Only Status Data)는 서로 관련된 변수들을 연속 레지스터 읽기(Contiguous Register Read)를 통해 효율적으로 가져올 수 있도록 구성하는 것이 좋다. 클라이언트가 전압, 전류, 충전 상태(State of Charge), 온도 및 운전 상태를 필요로 한다면 여러 개의 개별 요청 대신 소수의 홀딩 레지스터 읽기(Read Holding Registers) 또는 입력 레지스터 읽기(Read Input Registers) 트랜잭션으로 가져올 수 있도록 배치하는 것이 바람직하다. 효율적인 그룹화는 폴링 오버헤드(Polling Overhead), TCP 트랜잭션, 장치 처리 부하 및 네트워크 트래픽을 줄인다.

동일한 원칙은 쓰기 가능한 데이터(Writable Data)에도 적용된다. 함께 갱신되는 파라미터는 연속된 홀딩 레지스터에 배치하여 다중 레지스터 쓰기(Write Multiple Registers)를 통해 하나의 트랜잭션으로 전송할 수 있다. 그러나 그룹화는 단순히 주소 공간을 최소화하기보다는 의미적 연관성(Semantic Relationship)을 반영해야 한다. 서로 관련 없는 설정, 안전 관련 명령, 진단 데이터, 일시적인 제어 데이터를 하나의 쓰기 블록에 혼합하면 클라이언트 구현이 어려워지고 잘못된 쓰기의 영향도 커질 수 있다.

레지스터 맵 설계(Register-Map Design)에는 잘못된 주소(Invalid Address), 지원되지 않는 기능(Unsupported Function), 범위를 벗어난 값(Out-of-Range Value), 부적절한 운전 상태에서의 쓰기 요청에 대한 동작도 명시적으로 포함해야 한다. 서버는 처리할 수 없는 요청에 대해 모드버스 예외 코드(Modbus Exception Code)를 반환할 수 있으며, 응용 수준 상태 레지스터(Application-Level Status Register)를 통해 더 자세한 원인을 제공할 수 있다. 프로토콜 오류와 장치별 운전 오류를 분리하면 통신, 주소 지정, 파라미터 검증 또는 장비 상태 중 어디에서 문제가 발생했는지 판단하기 쉬워진다.

장비가 장기간 현장에서 사용되는 경우 버전 호환성(Version Compatibility)은 더욱 중요해진다. 기존 레지스터 주소와 의미는 가능한 한 안정적으로 유지하고, 새로운 변수는 예약된 영역 또는 새롭게 할당된 영역에 배치해야 한다. 장치 정보 블록(Device-Information Block)은 펌웨어 버전(Firmware Version), 하드웨어 개정판(Hardware Revision), 레지스터 맵 버전(Register-Map Version), 제품 식별자(Product Identifier), 기능 정보(Capability Information)를 제공할 수 있다. 이를 통해 클라이언트는 선택적 레지스터나 기능의 지원 여부를 사용 전에 확인할 수 있다.

로보틱스(Robotics)와 자율이동로봇(AMR) 응용에서 레지스터 맵은 충전기 상태(Charger Status), 배터리 정보(Battery Information), 컨베이어 핸드셰이크(Conveyor Handshake), 도킹 신호(Docking Signal), 자동문 또는 리프트 명령, 원격 입출력(Remote I/O) 상태, 전력 측정값, 환경 센서 및 공장 장비 인터페이스를 제공할 수 있다. 이러한 변수는 고주파 서보 제어(High-Frequency Servo Control)나 대용량 인지 데이터(Perception Data)가 아니라 장비 수준 통합(Equipment-Level Integration)을 표현하는 것이 적절하다.

견고한 레지스터 맵(Robust Register Map)은 궁극적으로 단순한 주소 목록이 아니라 안정적인 응용 프로그래밍 인터페이스(Application Programming Interface, API)로 취급해야 한다. 논리적 그룹화(Logical Grouping), 일관된 명명 규칙, 명확한 데이터 표현, 제어된 쓰기 접근(Controlled Write Access), 향후 확장을 위한 예약 공간, 문서화된 오류 동작 및 버전 관리를 적용하면 서로 다른 구현에서도 동일한 데이터를 일관되게 해석할 수 있다. 따라서 신중한 레지스터 맵 설계는 모드버스 TCP(Modbus TCP) 통합의 실질적인 상호운용성(Interoperability), 유지보수성(Maintainability), 장기 신뢰성(Long-Term Reliability)을 결정하는 핵심 요소이다.

## 03.04. Modbus TCP Security

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

모드버스 TCP(Modbus TCP)는 사이버보안(Cybersecurity)보다는 산업 자동화(Industrial Automation)에서의 상호운용성(Interoperability)과 단순성을 중심으로 설계되었다. 전통적인 모드버스 TCP 통신은 일반적인 TCP 포트 502(TCP Port 502) 트래픽에 대해 자체적인 암호화(Encryption), 강력한 인증(Authentication), 권한 부여(Authorization) 메커니즘을 제공하지 않는다. 따라서 장치는 문법적으로 유효한 요청을 수신하면 구현된 레지스터 권한에 따라 이를 처리할 수 있으므로, 네트워크 아키텍처와 외부 보안 제어가 안전한 구축의 핵심 요소가 된다.

이러한 보안 한계는 부분적으로 산업 제어 네트워크(Industrial Control Network)의 역사적인 운용 환경에서 비롯된다. 초기 모드버스 시스템은 장치와 통신 경로를 신뢰할 수 있다고 간주하는 물리적으로 격리되거나 엄격하게 통제된 환경에서 주로 구축되었다. 그러나 현대의 공장, 로봇, 자율이동로봇(AMR), 엣지 컴퓨터(Edge Computer), 원격 유지보수 시스템(Remote Maintenance System), IT/OT 통합은 연결성을 크게 증가시켰으므로 물리적 격리에만 의존하는 기존 가정은 더 이상 충분하지 않다.

기밀성(Confidentiality)은 기존 모드버스 TCP 메시지가 응용 계층 암호화(Application-Level Encryption) 없이 전송되기 때문에 중요한 보안 문제이다. 네트워크 트래픽을 관찰할 수 있는 공격자는 기능 코드(Function Code), 레지스터 주소(Register Address), 명령 값(Command Value), 측정값, 장치 상태 정보를 확인할 수 있다. 개별 값 자체는 중요하지 않아 보이더라도 반복적인 관찰을 통해 운전 순서, 장비 동작, 생산 조건 또는 제어 관계를 파악하여 추가 공격에 활용할 수 있다.

무결성(Integrity) 역시 중요하다. 모드버스 명령은 장치 데이터를 직접 변경할 수 있기 때문이다. 단일 코일 쓰기(Write Single Coil), 단일 레지스터 쓰기(Write Single Register), 다중 코일 쓰기(Write Multiple Coils), 다중 레지스터 쓰기(Write Multiple Registers)는 서버가 해당 기능을 지원할 경우 출력, 파라미터, 운전 모드 또는 명령을 변경할 수 있다. 권한이 없는 시스템이 네트워크에 접근하면 추가적인 접근 제한이나 보안 메커니즘이 없는 경우 악의적인 쓰기 요청으로 장비 동작을 변경할 가능성이 있다.

인증(Authentication)은 통신 상대가 실제로 허가된 시스템인지를 확인하는 문제를 다룬다. 표준 모드버스 TCP 통신은 기본적인 요청-응답 프로토콜(Request-Response Protocol)을 통해 일반적으로 클라이언트를 인증하지 않는다. 따라서 서버는 승인된 PLC나 엣지 컴퓨터와 동일한 TCP 서비스에 접근할 수 있는 다른 호스트를 구별하는 능력이 제한될 수 있다. 그러므로 안전한 시스템 설계에서는 IP 주소를 보유하거나 네트워크에 연결되어 있다는 사실만으로 충분한 권한이 있다고 판단해서는 안 된다.

네트워크 세분화(Network Segmentation)는 모드버스 TCP를 보호하기 위한 가장 중요한 실질적 방법 중 하나이다. 산업 장치는 일반적으로 기업 네트워크(Enterprise Network)나 공용 인터넷(Public Internet)에 직접 노출하기보다 통제된 운영기술 네트워크(Operational Technology Network, OT Network)에 배치해야 한다. VLAN, 산업용 방화벽(Industrial Firewall), 라우터(Router), 접근 제어 목록(Access-Control List), 보안 영역(Security Zone)을 사용하면 모드버스 연결을 시작할 수 있는 시스템을 제한하고 공격자가 중요 장비에 접근할 수 있는 경로를 줄일 수 있다.

일반적인 아키텍처에서는 기업 IT(Enterprise IT), 상위 감시 시스템(Supervisory System), 로봇 또는 AMR 네트워크, 기계 제어 네트워크(Machine-Control Network), 안전 관련 도메인(Safety-Related Domain)을 각각의 운용 역할에 따라 분리한다. 영역 간 통신은 제한 없는 스위칭 대신 정의된 게이트웨이(Gateway) 또는 방화벽을 통과하도록 구성한다. 이를 통해 모드버스 TCP 트래픽은 필요한 클라이언트와 서버 사이에서만 허용하고 목적지 주소, TCP 포트 502, 통신 방향 및 필요한 경우 응용 인식 필터링(Application-Aware Filtering)을 적용할 수 있다.

산업용 방화벽(Industrial Firewall)은 단순한 포트 필터링(Port Filtering)보다 세밀한 보호 기능을 제공할 수 있다. 일부 보안 장치는 모드버스 기능 코드(Function Code)를 이해하고 요청된 동작에 따라 규칙을 적용할 수 있다. 예를 들어 모니터링 시스템(Monitoring System)에는 레지스터 읽기 기능만 허용하면서 쓰기 기능은 차단할 수 있다. 이러한 프로토콜 인식 제어(Protocol-Aware Control)는 각 클라이언트에 필요한 동작만 허용하는 최소 권한 원칙(Principle of Least Privilege)을 적용하여 위험을 줄일 수 있다.

레지스터 맵 설계(Register-Map Design) 역시 보안에 기여한다. 읽기 전용 측정값과 상태 값은 쓰기 가능한 파라미터로 노출하지 않아야 하며, 중요한 명령은 일반적인 설정 데이터와 분리해야 한다. 서버는 쓰기를 허용하기 전에 주소, 데이터 범위, 운전 상태, 지원 기능 코드를 검증해야 한다. 명령-승인 메커니즘(Command-and-Acknowledgment Mechanism), 상태 기반 권한(State-Dependent Permission), 응용 계층 검증(Application-Level Validation)을 적용하면 예상하지 못한 요청이 즉시 위험한 물리적 동작을 발생시킬 가능성을 더욱 줄일 수 있다.

모드버스 보안(Modbus Security)은 현대적인 보안 메커니즘을 이용하여 모드버스 통신에 더욱 강력한 보호 기능을 제공하기 위한 확장 방식이다. 전송 계층 보안(Transport Layer Security, TLS)을 통합하여 통신을 보호하고 인증과 무결성을 위해 인증서 기반 메커니즘(Certificate-Based Mechanism)을 사용한다. 따라서 보안 모드버스 구축에서는 신뢰할 수 있는 네트워크 경계에만 의존하지 않고 인증되고 암호화된 세션(Authenticated Encrypted Session)을 설정할 수 있다. 다만 기존 모드버스 TCP 제품은 일반 통신만 구현할 수 있으므로 각 장치의 지원 여부를 확인해야 한다.

전송 계층 보안(TLS)은 종단점 사이의 통신을 암호화하여 기밀성(Confidentiality)을 보호하고, 전송 정보의 비인가 변경을 탐지하는 데 도움이 되는 무결성 메커니즘(Integrity Mechanism)을 제공할 수 있다. 인증서 기반 인증(Certificate-Based Authentication)을 이용하면 시스템이 통신 상대의 신원을 확인할 수도 있다. 그러나 이러한 기능을 적용하면 인증서 발급, 저장, 갱신, 폐기, 신뢰 설정(Trust Configuration), 펌웨어 지원 및 장치 수명주기 관리(Device Lifecycle Management)가 추가적인 공학적 관리 대상이 된다.

보안은 가용성(Availability)도 고려해야 한다. 산업 통신 시스템은 과도한 연결, 비정상 요청(Malformed Request), 지나치게 공격적인 폴링(Aggressive Polling), 의도적인 서비스 거부(Denial-of-Service) 트래픽의 영향을 받을 수 있기 때문이다. 임베디드 모드버스 서버(Embedded Modbus Server)는 CPU, 메모리, 소켓(Socket) 용량 또는 트랜잭션 처리 자원이 제한될 수 있다. 속도 제한(Rate Limiting), 연결 수 제한, 트래픽 모니터링, 네트워크 격리 및 적절한 클라이언트 폴링 주기를 적용하면 통신 과부하가 장비 운전을 방해하는 위험을 줄일 수 있다.

모드버스 트래픽은 비교적 예측 가능한 패턴을 가지므로 모니터링(Monitoring)이 특히 유용하다. PLC는 동일한 레지스터 영역을 반복적으로 읽고 특정 운전 조건에서만 쓰기 명령을 수행할 수 있다. 따라서 네트워크 모니터링 시스템(Network Monitoring System)은 비정상적인 클라이언트, 예상하지 못한 기능 코드, 비정상적인 폴링 속도, 새로운 통신 경로 또는 거의 변경되지 않는 레지스터에 대한 쓰기를 식별할 수 있다. 이러한 이상 현상(Anomaly)이 반드시 공격을 의미하는 것은 아니지만 조사와 운전 진단을 위한 중요한 지표가 될 수 있다.

로깅(Logging)은 중요한 통신 및 응용 이벤트를 기록함으로써 네트워크 모니터링을 보완해야 한다. 주요 기록에는 지원되는 경우의 인증 이벤트(Authentication Event), 설정 변경, 중요 레지스터 쓰기, 거부된 명령, 통신 장애, 펌웨어 변경, 보안 정책 위반(Security-Policy Violation) 등이 포함될 수 있다. 정확한 타임스탬프(Timestamp)와 동기화된 시계(Synchronized Clock)를 사용하면 PLC, 로봇, 게이트웨이, 엣지 컴퓨터, 스위치 및 상위 시스템에서 발생한 이벤트를 엔지니어가 시간 순서에 따라 재구성하는 데 도움이 된다.

원격 접근(Remote Access)은 TCP 포트 502를 인터넷에 직접 노출하면 불필요한 위험이 발생하므로 추가적인 보호가 필요하다. 원격 엔지니어링(Remote Engineering)이나 유지보수는 일반적으로 보안 VPN 인프라(Secure VPN Infrastructure), 인증된 점프 호스트(Authenticated Jump Host), 관리형 원격 접근 게이트웨이(Managed Remote-Access Gateway) 또는 이와 동등한 보호 채널을 통해 수행해야 한다. 접근 권한은 전체 산업 네트워크를 개방하는 대신 필요한 사용자, 장치, 시간 범위 및 네트워크 목적지로 제한해야 한다.

장치 강화(Device Hardening)는 모드버스 TCP 보안의 또 다른 계층이다. 가능한 경우 사용하지 않는 서비스와 포트를 비활성화하고, 기본 자격 증명(Default Credential)을 변경하며, 제조사의 지침에 따라 펌웨어를 유지하고, 관리 인터페이스(Management Interface)에 대한 접근을 제한해야 한다. 네트워크 설정은 운용 도메인 사이의 불필요한 라우팅을 방지해야 한다. 또한 장애나 보안 사고 이후 설정 백업, 펌웨어 이미지, 인증서 및 레지스터 맵 정의를 복원할 수 있도록 복구 절차(Recovery Procedure)를 고려해야 한다.

로보틱스(Robotics)와 자율이동로봇(AMR) 시스템에서 모드버스 TCP는 엣지 컴퓨터나 PLC를 충전기(Charger), 컨베이어(Conveyor), 자동문(Door), 리프트(Lift), 원격 입출력(Remote I/O), 전력계(Power Meter), 공장 장비(Factory Equipment)와 연결할 수 있다. 모드버스가 고주파 모션 제어(High-Frequency Motion Control)에 사용되지 않더라도 침해된 명령 경로(Command Path)는 실제 물리적 공정에 영향을 줄 수 있다. 따라서 각각의 쓰기 가능 인터페이스가 가져올 결과를 고려하고 통신 장애나 비인가 명령이 독립적인 안전 메커니즘(Safety Mechanism)을 우회하지 못하도록 설계해야 한다.

따라서 모드버스 TCP 보안(Modbus TCP Security)은 하나의 보호 기술에 의존하기보다 심층 방어(Defense in Depth)를 통해 구현하는 것이 바람직하다. 네트워크 세분화는 접근 가능 범위를 제한하고, 방화벽은 통신을 통제하며, 프로토콜 인식 정책은 허용되는 동작을 제한한다. 레지스터 검증은 응용 동작을 보호하고, TLS 기반 모드버스 보안은 인증과 암호화를 제공하며, 모니터링은 비정상적인 활동을 탐지하는 데 도움을 준다. 이러한 계층을 장치 강화 및 독립적인 기능 안전(Functional Safety) 메커니즘과 결합하면 현대적인 산업, 로보틱스 및 AMR 네트워크에 모드버스 TCP를 보다 안전하게 통합할 수 있다.

## 03.05. Modbus TCP in Fleet Management

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

모드버스 TCP(Modbus TCP)는 자율이동로봇 플릿 관리(AMR Fleet Management) 아키텍처에서 장비 수준 통합 프로토콜(Equipment-Level Integration Protocol)로 활용될 수 있으며, 특히 이동 로봇이 기존 산업 인프라와 상호작용해야 하는 경우 유용하다. 플릿 관리 시스템(Fleet Management System)은 일반적으로 미션(Mission), 교통 제어(Traffic), 충전(Charging), 자원 할당(Resource Allocation)을 조정하며, 모드버스 TCP는 PLC, 충전기, 컨베이어, 자동문, 리프트, 원격 입출력(Remote I/O) 및 기타 공장 장비와 연동하기 위한 단순한 레지스터 기반 인터페이스(Register-Based Interface)를 제공한다.

플릿 관리 시스템(Fleet Management System)은 일반적으로 풍부한 로봇 텔레메트리(Robot Telemetry), 지도(Map), 궤적(Trajectory), 인지 데이터(Perception Data), 복잡한 미션 정보를 교환하기 위한 주요 상위 통신 수단으로 모드버스 TCP를 사용하지 않는 것이 바람직하다. 이러한 기능은 로보틱스 지향 미들웨어(Robotics-Oriented Middleware)나 응용 인터페이스(Application Interface)를 통해 처리하는 것이 적합하다. 모드버스 TCP는 대신 산업 자동화 장치와 간결한 장비 상태, 명령, 승인(Acknowledgment), 인터록(Interlock), 공정 변수(Process Variable)를 교환하는 하위 통합 계층에서 활용된다.

일반적인 아키텍처에서는 플릿 관리자(Fleet Manager), 로봇 관리 서버(Robot Management Server) 또는 통합 게이트웨이(Integration Gateway)를 공장 자동화 장비와 연결된 이더넷 네트워크(Ethernet Network)에 배치한다. 시스템 설계에 따라 플릿 소프트웨어가 모드버스 TCP 클라이언트(Modbus TCP Client)로 동작하여 여러 서버와 주기적으로 통신할 수 있다. PLC, 충전 제어기, 컨베이어 제어기, 원격 입출력 모듈, 자동문, 리프트 및 공정 장비는 정의된 코일(Coils)과 레지스터(Registers)를 통해 운전 정보를 제공할 수 있다.

반대로 플릿 통합 게이트웨이(Fleet Integration Gateway)가 선택된 로봇 또는 플릿 정보를 모드버스 TCP 서버(Modbus TCP Server)로 제공하는 구성도 가능하다. 이 경우 공장 PLC는 내부 플릿 관리 API(Fleet-Management API)를 이해하지 않고도 플릿 상태 레지스터를 읽거나 사전에 정의된 명령 레지스터에 값을 기록할 수 있다. 이를 통해 로보틱스 도메인(Robotics Domain)과 기존 자동화 도메인(Automation Domain) 사이에 프로토콜 경계(Protocol Boundary)를 형성하고 기존 PLC 프로그램이 익숙한 모드버스 기능 코드와 레지스터 맵을 통해 AMR과 연동할 수 있다.

로봇 작업이 외부 장비의 상태에 의존하는 경우 미션 실행(Mission Execution)에 모드버스 TCP를 활용할 수 있다. 예를 들어 AMR이 컨베이어 스테이션(Conveyor Station)에 접근할 때 도킹(Docking) 전에 컨베이어가 준비되었는지 확인해야 할 수 있다. 플릿 시스템은 준비 상태 레지스터를 읽고 적절한 명령을 통해 스테이션을 예약한 뒤 AMR을 배차하고, 물품 이송을 허용하기 전에 추가적인 핸드셰이크 상태(Handshake State)를 기다릴 수 있다. 따라서 레지스터 인터페이스는 미션 계획기 자체가 아니라 더 큰 미션 상태 시퀀스(Mission-State Sequence)의 일부가 된다.

컨베이어 통합(Conveyor Integration)은 일반적으로 플릿 또는 로봇 시스템과 컨베이어 PLC 사이의 핸드셰이크(Handshake)를 사용한다. 대표적인 정보에는 스테이션 사용 가능(Station Available), 로봇 도착(Robot Arrived), 도킹 완료(Docking Complete), 이송 요청(Transfer Request), 컨베이어 운전 중(Conveyor Running), 이송 완료(Transfer Complete), 고장(Fault), 리셋(Reset) 상태 등이 포함될 수 있다. 이러한 변수는 코일, 디스크리트 입력(Discrete Inputs), 또는 레지스터 내부의 비트 필드(Bit Field)로 표현할 수 있으며, 개별 불리언 신호만으로는 독립적으로 동작하는 시스템 사이의 올바른 협조를 보장할 수 없으므로 명확한 상태 전이 시퀀스(State Transition Sequence)가 중요하다.

자동문(Automatic Door)은 또 다른 일반적인 플릿 관리 인터페이스를 제공한다. AMR이 통제 구역에 진입하기 전에 플릿 시스템이 자동문 열기 요청을 보내고, 로봇이 계속 이동하도록 허가하기 전에 문 열림 또는 준비 상태를 확인할 수 있다. 추가 레지스터는 닫힘(Closed), 열리는 중(Opening), 닫히는 중(Closing), 장애물 감지(Obstruction), 고장, 수동 모드(Manual Mode), 접근 허가(Access Permission) 등을 나타낼 수 있다. 내비게이션(Navigation)은 문 열기 명령을 전송했다는 사실만으로 물리적 통로가 즉시 확보되었다고 가정해서는 안 되며, 독립적으로 보고되는 장비 상태를 통해 이를 확인해야 한다.

리프트 통합(Lift Integration)은 여러 운전 상태를 가진 공유 자원(Shared Resource)이므로 보다 광범위한 조정이 필요하다. 모드버스 레지스터는 층 위치(Floor Position), 문 상태(Door Status), 사용 가능 여부(Availability), 점유 상태(Occupancy), 예약(Reservation), 요청 목적지(Requested Destination), 운전 모드, 고장 등을 표현할 수 있다. 플릿 관리자는 리프트를 예약하고 필요한 층을 요청하며 문 상태를 확인한 후 AMR의 진입을 허용하고 목적지 이동을 요청한 다음, 안전한 진출을 확인한 뒤 다른 미션이 사용할 수 있도록 자원을 해제할 수 있다.

충전 인프라(Charging Infrastructure) 역시 모드버스 TCP를 통해 통합할 수 있다. 충전 제어기(Charging Controller)는 충전기 사용 가능 여부, 연결 상태(Connection State), 출력 전압, 전류, 충전 상태, 에너지 정보, 온도, 경보 상태 및 고장 코드를 제공할 수 있다. 플릿 관리자는 이러한 값을 로봇의 배터리 정보와 결합하여 사용 가능한 충전기를 선택하고 충전 미션을 계획할 수 있다. 장치가 지원하는 경우 쓰기 가능한 레지스터를 통해 활성화(Enable), 리셋, 운전 모드 또는 충전 승인(Charging Authorization)과 같은 제어 기능을 제공할 수도 있다.

여러 로봇이 동일한 물리적 장비를 요청할 수 있으므로 플릿 관리에서는 자원 중재(Resource Arbitration)가 필요하다. 모드버스 자체는 플릿 수준의 예약 알고리즘(Fleet-Level Reservation Algorithm)을 제공하지 않으므로 소유권과 스케줄링(Scheduling)은 플릿 관리자 또는 다른 상위 제어기가 구현해야 한다. 리프트, 충전기, 컨베이어 스테이션 또는 제한 통로와 같은 자원에는 일반적으로 하나의 권위 있는 자원 관리자(Authoritative Resource Manager)를 두어 여러 로봇이나 클라이언트의 충돌하는 명령을 방지하고 자원 점유 상태를 일관되게 관리해야 한다.

따라서 레지스터 맵 설계(Register-Map Design)는 플릿 통합에서 매우 중요하다. 장비 인터페이스는 명령(Commands), 상태(Status), 승인(Acknowledgments), 경보(Alarms), 측정값(Measurements), 설정 정보(Configuration Information)를 구분해야 한다. 관련 값은 플릿 시스템이 효율적으로 읽을 수 있도록 연속된 주소 블록(Contiguous Address Block)으로 구성하는 것이 좋다. 명령 레지스터는 읽기 전용 상태 정보와 분리하고 데이터 형식, 스케일링(Scaling), 바이트 순서(Byte Order), 유효 범위, 갱신 동작 및 지원되는 모드버스 기능 코드를 장비 인터페이스 전반에 걸쳐 일관되게 문서화해야 한다.

견고한 핸드셰이크(Robust Handshake)는 명령 승인(Command Acceptance)과 실제 물리적 완료(Physical Completion)를 구분해야 한다. 예를 들어 도킹 준비 명령을 기록했다고 해서 스테이션이 준비되었다는 의미는 아니며, 자동문 열기 요청을 했다고 해서 문이 실제로 열린 것은 아니다. 명령(Command), 승인(Accepted), 처리 중(Busy), 완료(Completed), 고장(Fault) 상태를 각각 분리하면 플릿 관리자가 실제 동작 진행 상황을 추적할 수 있다. 트랜잭션 식별자(Transaction Identifier)나 응용 계층 시퀀스 카운터(Application-Level Sequence Counter)를 사용하면 이전 상태가 새로운 요청에 대한 확인으로 잘못 해석되는 것을 방지하는 데 도움이 된다.

외부 장비가 응답하지 않거나 중간 상태에 머무를 수 있으므로 타임아웃(Timeout)과 복구 동작(Recovery Behavior)을 정의해야 한다. 플릿 시스템은 통합 오류(Integration Fault)를 선언하기 전에 승인과 완료 상태를 얼마나 오래 기다릴 것인지 알고 있어야 한다. 응용 환경에 따라 복구 과정에는 요청 재시도, 자원 예약 해제, 다른 자원 선택, 해당 미션 중단, 운영자 지원 요청(Operator Assistance), 또는 로봇을 안전한 대기 위치(Safe Waiting Location)로 이동시키는 동작이 포함될 수 있다.

폴링 전략(Polling Strategy)은 플릿 응답성과 네트워크 부하 모두에 영향을 준다. 빠르게 변화하는 핸드셰이크 상태는 비교적 빈번한 읽기가 필요할 수 있지만 에너지 카운터, 온도, 진단 및 유지보수 정보는 낮은 빈도로 갱신해도 된다. 함께 사용되는 레지스터는 적은 수의 트랜잭션으로 가져올 수 있도록 그룹화해야 한다. 대규모 플릿에서는 각 로봇이 장비 상태를 개별적으로 반복 폴링하기보다 중앙 게이트웨이(Centralized Gateway) 또는 플릿 서버가 장비 상태를 한 번 수집하고 내부적으로 배포하는 구조를 통해 불필요한 통신을 줄일 수 있다.

로봇과 산업 장치의 수가 증가할수록 네트워크 아키텍처(Network Architecture)는 더욱 중요해진다. 관리형 이더넷 스위치(Managed Ethernet Switch), VLAN, 라우팅 정책(Routing Policy), 산업용 방화벽(Industrial Firewall)을 사용하여 플릿 트래픽을 기계 제어, 기업 IT, 인지(Perception) 및 기타 통신 도메인과 분리할 수 있다. 모든 AMR이 모든 공장 장비와 직접 통신하도록 허용하는 대신 승인된 플릿 서버, PLC 또는 게이트웨이에만 모드버스 TCP 접근을 허용하면 보안과 시스템 관리를 모두 단순화할 수 있다.

플릿 수준의 모드버스 명령은 공유 물리 자원(Shared Physical Resource)에 영향을 줄 수 있으므로 보안(Security)이 특히 중요하다. 자동문, 컨베이어, 충전기 또는 리프트에 대한 비인가 쓰기(Unauthorized Write)는 하나의 장치뿐 아니라 여러 로봇의 운용을 방해할 수 있다. 따라서 네트워크 세분화(Network Segmentation), 접근 제어(Access Control), 프로토콜 인식 필터링(Protocol-Aware Filtering), 장치 강화(Device Hardening), 로깅(Logging), 보안 원격 접근(Secure Remote Access)을 통해 통합 경로를 보호해야 한다. 지원되는 경우 TLS 기반 모드버스 보안(Modbus Security)은 기존 모드버스 TCP보다 강력한 인증, 무결성 및 기밀성을 제공할 수 있다.

운영 모니터링(Operational Monitoring)은 모드버스 통신 정보와 플릿 수준의 상황 정보(Fleet-Level Context)를 결합해야 한다. 단순한 통신 타임아웃은 장치가 응답하지 않았다는 사실만 나타내지만, 플릿 상황 정보는 어떤 미션, 로봇, 스테이션 또는 자원이 영향을 받았는지 식별할 수 있다. 따라서 로그는 장비 주소, 레지스터 동작, 명령, 승인, 고장 코드, 로봇 식별자(Robot Identifier), 미션 식별자(Mission Identifier), 타임스탬프(Timestamp)를 연계하여 기록해야 하며, 이를 통해 엔지니어가 간헐적인 통합 장애를 진단할 때 전체 상호작용 과정을 재구성할 수 있다.

시스템 안전 아키텍처(System Safety Architecture)에서 요구되는 경우 안전 기능(Safety Function)은 일반적인 플릿 관리 통신과 독립적으로 유지되어야 한다. 모드버스 TCP는 운전 상태를 조정할 수 있지만 플릿 명령이 안전 등급 센서(Safety-Rated Sensor), 비상 정지 회로(Emergency-Stop Circuit), 안전 PLC(Safety PLC), 인증된 안전 네트워크(Certified Safety Network)를 대체해서는 안 된다. 예를 들어 자동문이나 컨베이어가 논리적으로 준비되었다는 확인 정보는 미션 시퀀싱에 활용할 수 있지만, 위험한 물리적 동작을 방지하는 책임은 독립적인 안전 메커니즘이 담당해야 한다.

따라서 AMR 플릿에서 모드버스 TCP(Modbus TCP)는 플릿 지능(Fleet Intelligence)과 기존 산업 장비(Conventional Industrial Equipment)를 연결하는 브리지(Bridge)로 사용할 때 가장 효과적이다. 플릿 관리자는 미션 스케줄링(Mission Scheduling), 자원 중재, 교통 조정(Traffic Coordination), 충전 의사결정(Charging Decision), 복구 로직(Recovery Logic)을 수행하고, 모드버스 TCP는 표준화된 기능 코드와 레지스터 맵을 통해 간결한 장비 명령과 상태를 제공한다. 이러한 역할 분리를 통해 모든 산업 장치가 로보틱스 전용 통신 미들웨어를 구현하지 않아도 현대적인 로봇 플릿을 기존 공장 인프라와 체계적으로 통합할 수 있다.
