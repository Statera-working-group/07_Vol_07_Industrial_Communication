**Volume 07. Industrial Communication**

# Chapter 01. RS232/RS422

## 01.01. RS232 Signal Level and Timing

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

RS-232는 공통 신호 접지(signal ground)를 기준으로 하는 전압 레벨(voltage level)을 통해 이진 정보를 표현하는 단일 종단 직렬 통신(single-ended serial communication) 표준이다. 일반적인 디지털 논리(digital logic)에서는 양의 전압이 보통 논리 1(logic 1)을 의미하지만, RS-232는 반전된 규칙(inverted convention)을 사용한다. 충분히 낮은 음의 전압은 이진수 1에 해당하는 마크 상태(MARK state)를 나타내며, 충분히 높은 양의 전압은 이진수 0에 해당하는 스페이스 상태(SPACE state)를 나타낸다. 이러한 전기적 규칙(electrical convention)은 RS-232 인터페이스(interface)를 정의하는 핵심적인 특징 중 하나이다.

공식적인 RS-232 전기 규격(electrical specification)은 케이블 손실(cable loss), 잡음(noise), 장비 간 전기적 차이가 존재하더라도 안정적인 통신이 가능하도록 비교적 넓은 전압 여유(voltage margin)를 제공한다. 수신기(receiver)에서는 대략 −3 V보다 낮은 전압을 마크(MARK)로 해석하고, +3 V보다 높은 전압을 스페이스(SPACE)로 해석한다. −3 V에서 +3 V 사이의 영역은 의도적으로 미정의 영역(undefined region)으로 두어 두 유효 신호 영역 사이에 잡음 여유(noise margin)를 제공한다.

송신기(transmitter)는 일반적으로 수신기 임계값(receiver threshold)보다 충분히 큰 신호 진폭(signal amplitude)을 생성하며, 구현 방식에 따라 대략 ±5 V에서 ±15 V 수준의 전압을 사용한다. 과거의 장비는 비교적 높은 양전압과 음전압을 자연스럽게 제공하는 전원 공급 장치(power supply)를 사용했지만, 현대 시스템에서는 저전압 논리 전원으로부터 적절한 RS-232 전압을 생성하는 차지 펌프 송수신기(charge-pump transceiver)를 많이 사용한다. 따라서 실제 송신 전압의 크기는 구현 방식에 따라 달라질 수 있지만 호환되는 수신기의 전기적 요구 조건을 만족해야 한다.

RS-232 전압 레벨은 TTL 또는 CMOS 논리 레벨(logic level)과 근본적으로 다르기 때문에 RS-232 커넥터(connector)를 마이크로컨트롤러 UART에 직접 연결해서는 안 된다. UART는 5 V, 3.3 V, 1.8 V 또는 다른 논리 전압에서 동작할 수 있으며 일반적으로 비반전 논리 신호(non-inverted logic signaling)를 사용한다. RS-232 송수신기(transceiver)는 전압 변환(voltage translation)과 논리 반전(logical inversion)을 동시에 수행하여 컨트롤러(controller), PLC, 임베디드 컴퓨터(embedded computer), 로봇 서브시스템(robot subsystem)의 UART가 RS-232 장치와 안전하게 통신할 수 있도록 한다.

RS-232 통신은 일반적으로 비동기식(asynchronous)으로 동작하므로 송신기와 수신기가 별도의 클록 선(clock line)을 공유하지 않는다. 두 통신 장치는 사전에 동일한 보레이트(baud rate)와 프레임 형식(frame format)으로 설정된다. 문자가 전송되지 않는 동안에는 통신선이 마크 상태(MARK state)를 유지한다. 송신기가 통신선을 스페이스 상태(SPACE state)로 변경하여 시작 비트(start bit)를 생성하면 전송이 시작되며, 수신기는 이를 통해 새로운 직렬 문자(serial character)의 시작을 인식하고 내부 샘플링 과정(sampling process)을 동기화한다.

수신기는 시작 비트 전이(start-bit transition)를 감지한 후 설정된 비트 주기(bit period)에 따라 신호를 샘플링한다. 9600 보드(baud)에서는 하나의 비트가 약 104.17 마이크로초(microseconds)를 차지하며, 115200 보드에서는 비트 주기가 약 8.68 마이크로초이다. 수신기 하드웨어(receiver hardware)는 일반적으로 공칭 보레이트(nominal baud rate)보다 훨씬 빠른 내부 클록(internal clock)을 사용하여 시작 전이를 감지하고, 타이밍 불확실성(timing uncertainty)의 영향을 최소화할 수 있도록 각 데이터 비트(data bit)의 중앙 부근에서 신호를 샘플링한다.

일반적인 비동기 RS-232 프레임(asynchronous RS-232 frame)은 하나의 시작 비트(start bit), 여러 개의 데이터 비트(data bits), 선택적인 패리티 비트(parity bit), 그리고 하나 이상의 정지 비트(stop bits)로 구성된다. 널리 사용되는 설정은 8N1이며, 이는 8개의 데이터 비트, 패리티 없음(no parity), 1개의 정지 비트를 의미한다. 데이터 비트는 일반적으로 최하위 비트 우선(least significant bit first)으로 전송된다. 정지 비트는 통신선을 다시 마크 상태(MARK state)로 복귀시키며, 필요한 정지 구간이 완료되면 다음 시작 비트가 즉시 이어질 수 있다.

보레이트(baud rate)는 신호 전송 속도(signaling rate)를 나타내지만 실제 유효 데이터 처리량(useful data throughput)은 프레임 구성 비트가 전송 시간을 사용하기 때문에 이보다 낮다. 8N1 형식에서는 8비트 페이로드 문자(payload character) 하나를 전송하기 위해 시작 비트 1개, 데이터 비트 8개, 정지 비트 1개를 포함하여 총 10비트가 필요하다. 따라서 9600 보드 연결에서는 문자를 연속적으로 전송할 경우 이론적으로 초당 약 960개의 8비트 문자를 전달할 수 있다. 프로토콜 헤더(protocol header), 구분자(delimiter), 응답(acknowledgment), 응용 프로그램 처리(application processing)는 실제 유효 처리량을 더욱 감소시킬 수 있다.

비동기 통신(asynchronous communication)은 하나의 문자가 전송되는 동안 송신기와 수신기가 충분히 유사한 클록을 유지하는 것에 의존하므로 타이밍 정확도(timing accuracy)가 중요하다. 수신기는 각각의 시작 비트를 감지할 때 다시 동기화되므로 작은 클록 주파수 차이(clock-frequency difference)는 일반적으로 허용된다. 그러나 보레이트 불일치(baud-rate mismatch)가 지나치게 크면 샘플링 지점(sampling point)이 연속되는 비트의 중앙에서 점차 벗어나게 된다. 결국 수신기가 잘못된 논리 상태를 샘플링하여 문자 손상, 패리티 오류(parity error), 프레이밍 오류(framing error)가 발생할 수 있다.

정지 비트 구간(stop-bit interval)은 중요한 타이밍 검사 기능도 제공한다. 수신기가 정지 비트를 예상하는 시점에는 마크 상태(MARK state)가 관찰되어야 한다. 예상된 샘플링 시점에도 신호가 스페이스 상태(SPACE state)에 있다면 UART는 프레이밍 오류(framing error)를 보고할 수 있다. 이러한 오류는 잘못된 보레이트 설정, 서로 다른 프레임 설정, 심각한 전기적 잡음(electrical noise), 케이블 손상 또는 타이밍 불안정성(timing instability)을 의미할 수 있다. 따라서 프레이밍 오류 통계는 산업용 직렬 링크(industrial serial link)의 문제를 진단할 때 유용한 정보를 제공한다.

신호 품질(signal quality)은 공칭 전압(nominal voltage)뿐만 아니라 케이블 정전용량(cable capacitance), 소스 임피던스(source impedance), 접지(grounding), 전자기 간섭(electromagnetic interference)의 영향도 받는다. 케이블 길이가 증가하면 전기적 전이(electrical transition)가 느려지고 파형이 수신기 임계값을 통과하는 데 더 많은 시간이 필요할 수 있다. 높은 보레이트에서는 사용 가능한 비트 주기가 짧아져 느린 신호 에지(slow edge)와 왜곡(distortion)에 대한 허용 범위가 감소한다. 따라서 실제 RS-232 시스템에서는 보레이트와 케이블 길이를 서로 독립적인 요소로 보기보다 통신 거리와 신호 속도 사이의 절충 관계(trade-off)로 고려해야 한다.

RS-232는 단일 종단 방식(single-ended)이므로 모든 신호 전압이 공통 기준 도체(reference conductor)를 기준으로 해석된다. 두 장비 사이에 존재하는 접지 전위차(ground potential difference)는 원하지 않는 신호 전압으로 직접 나타날 수 있다. 모터(motor), 인버터(inverter), 접촉기(contactor), 스위칭 전원 공급 장치(switching power supply), 긴 케이블 경로가 존재하는 산업 환경에서는 이러한 특성 때문에 RS-232가 RS-422나 RS-485와 같은 차동 인터페이스(differential interface)보다 공통 모드 외란(common-mode disturbance)에 취약하다. 따라서 적절한 접지, 케이블 배선, 차폐(shielding), 절연(isolation)이 중요한 설계 요소가 될 수 있다.

또한 RS-232 전기 표준(electrical standard)은 UART가 구현하는 상위 수준의 직렬 데이터 형식(serial data format)과 구분하여 이해해야 한다. RS-232는 주로 전기적 인터페이스 특성과 관련 제어 신호(control signal)를 규정하는 반면, UART는 비동기식 시작 비트, 데이터 비트, 패리티, 정지 비트를 생성하고 해석한다. 응용 시스템은 이 직렬 데이터 스트림(serial stream) 위에 자체적인 프로토콜 구조(protocol structure)를 구성한다. 예를 들어 로봇 센서(robot sensor)는 물리적 전기 인터페이스가 RS-232로 변환된 UART를 통해 ASCII 명령이나 바이너리 패킷(binary packet)을 전송할 수 있다.

전통적인 RS-232 인터페이스는 송신 데이터와 수신 데이터 외에도 여러 제어선(control line)을 포함할 수 있다. RTS, CTS, DTR, DSR, DCD, RI와 같은 신호는 역사적으로 데이터 단말 장비(data terminal equipment)와 모뎀(modem) 사이의 통신에서 중요한 역할을 했다. 현대의 임베디드 및 로봇 응용에서는 TX, RX, 신호 접지(signal ground)만 사용하는 경우가 많으며, 일부 시스템은 하드웨어 흐름 제어(hardware flow control)를 위해 RTS와 CTS를 유지한다. 이러한 제어 신호의 전기적 극성(electrical polarity) 역시 일반적인 MCU 논리가 아니라 RS-232 신호 규칙을 따른다.

RS-232 인터페이스를 디버깅(debugging)할 때는 전압과 타이밍을 함께 확인해야 한다. 소프트웨어에서 문자가 정상적으로 표시되는지만 확인하면 물리 계층(physical layer)의 잠재적인 문제를 놓칠 수 있다. 오실로스코프(oscilloscope)를 사용하면 유휴 상태 극성(idle polarity), 전압 진폭, 신호 전이 품질, 시작 비트 타이밍, 개별 비트 주기, 정지 비트 동작을 확인할 수 있다. 논리 분석기(logic analyzer)는 전압 변환 이후의 신호 분석에 유용하며, RS-232 측을 직접 측정할 때에는 양극성 신호 레벨(bipolar signal level)에 적합한 측정 장비와 연결 방법이 필요하다.

RS-232는 구조가 단순하고 기술적으로 성숙했으며 비용이 낮고 문제 진단이 쉽기 때문에 산업 통신(industrial communication)에서 여전히 유용하게 사용된다. 설정 포트(configuration port), 레거시 PLC 장비(legacy PLC equipment), 모터 컨트롤러(motor controller), 실험실 계측기(laboratory instrument), 바코드 리더(barcode reader), 산업용 컴퓨터(industrial computer), 로봇 서브시스템 및 서비스 인터페이스(service interface)는 주변 시스템이 이더넷(Ethernet)이나 필드버스(fieldbus)를 사용하더라도 RS-232를 제공할 수 있다. 따라서 RS-232의 신호 레벨과 타이밍을 이해하는 것은 이후 RS-422, RS-485, 모드버스 RTU(Modbus RTU), 그리고 더욱 발전된 산업 통신 구조(industrial communication architecture)를 학습하기 위한 중요한 기초가 된다.

## 01.02. RS422 Differential Signaling

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

RS-422는 더 긴 거리와 전기적 잡음(electrical noise)이 많은 환경에서 전송 신뢰성을 향상시키기 위해 설계된 직렬 통신 전기 표준(serial communication electrical standard)이다. RS-422의 가장 중요한 특징은 차동 신호 방식(differential signaling)으로, 각각의 논리 신호(logical signal)를 접지를 기준으로 하는 하나의 신호선이 아니라 두 개의 도체(conductor)로 구성된 한 쌍을 통해 전송한다. 수신기(receiver)는 두 도체 사이의 전압 차이(voltage difference)를 이용하여 데이터 상태를 판단하기 때문에 외부에서 유입되는 전기적 잡음에 대한 저항성이 크게 향상된다.

차동 쌍(differential pair)을 구성하는 두 도체는 서로 상보적인 전기 상태(complementary electrical state)를 전달한다. 송신기(transmitter)가 한쪽 도체를 더 높은 전압으로 구동하면 다른 도체는 더 낮은 전압으로 구동되어 두 도체 사이에 차동 전압(differential voltage)이 형성된다. 전송되는 논리 상태가 변경되면 이 전압 차이의 극성(polarity)도 반전된다. 따라서 수신기는 각 도체의 절대 전압보다는 두 전압 사이의 상대적인 차이를 판단하며, 이를 개념적으로 Vdiff = VA − VB와 같이 표현할 수 있다.

이러한 동작 원리는 RS-232에서 사용하는 단일 종단 신호 방식(single-ended signaling)과 근본적으로 다르다. RS-232에서는 수신기가 공통 신호 접지(common signal ground)를 기준으로 신호 도체의 전압을 해석하기 때문에 접지 전위차(ground potential difference)나 결합된 전기적 간섭(electrical interference)이 수신 신호를 직접 교란할 수 있다. 반면 RS-422는 두 신호 도체 사이의 차이를 사용하기 때문에 두 도체에 유사하게 영향을 주는 외란(disturbance)을 차동 수신기(differential receiver)가 상당 부분 제거할 수 있다.

이러한 잡음 제거 능력은 공통 모드 동작(common-mode behavior)에서 비롯된다. 전자기적 외란(electromagnetic disturbance)이 두 전선에 거의 동일한 불필요한 전압을 유도하면 두 도체의 전압은 함께 변화하지만 두 전압의 차이는 거의 변하지 않는다. 수신기는 주로 이 전압 차이에 반응하므로 상당한 공통 모드 잡음(common-mode noise)이 상쇄된다. 이러한 특성으로 인해 차동 신호 방식은 모터(motor), 가변 주파수 드라이브(variable-frequency drive), 스위칭 전원 공급 장치(switching power supply), 릴레이(relay), 접촉기(contactor) 등 산업용 잡음원이 존재하는 환경에서 특히 유용하다.

RS-422 전송에는 일반적으로 연선(twisted pair)이 사용되는데, 전선을 꼬아 배치하면 두 도체가 유사한 전자기장(electromagnetic field)에 노출되도록 만들 수 있기 때문이다. 케이블을 따라 두 전선의 물리적 위치가 반복적으로 서로 바뀌면서 외부에서 유도되는 간섭이 두 도체에 보다 균등하게 영향을 주게 된다. 이는 공통 모드 잡음 제거 성능을 향상시키고 자기 결합(magnetic coupling)의 영향을 감소시킨다. 따라서 연선 구조는 차동 신호 방식이 제공하는 전기적 장점을 효과적으로 활용하기 위한 중요한 물리적 요소이다.

RS-422는 일반적으로 송신과 수신에 별도의 차동 쌍을 사용하여 전이중 통신(full-duplex communication)을 지원한다. 하나의 차동 쌍은 첫 번째 장치에서 두 번째 장치로 데이터를 전송하고, 다른 차동 쌍은 반대 방향으로 데이터를 전송한다. 두 방향이 서로 독립된 전기적 경로를 사용하기 때문에 양쪽 장치는 동시에 데이터를 전송할 수 있다. 이는 여러 장치가 하나의 차동 쌍을 공유하고 통신 매체 접근을 조정해야 하는 RS-485의 일반적인 2선식 반이중 네트워크(two-wire half-duplex network)와 차이가 있다.

RS-422의 전기적 구조(electrical architecture)는 일반적으로 하나의 드라이버(driver)가 하나 이상의 수신기와 통신하는 형태를 기반으로 한다. 따라서 하나의 송신기는 인터페이스의 전기적 부하 한계(electrical loading limit) 내에서 여러 수신 장치에 신호를 전달할 수 있다. 이러한 특성은 하나의 컨트롤러(controller)가 여러 수신 노드(receiving node)에 직렬 정보를 제공해야 하는 산업 장비에서 유용하다. 그러나 RS-422를 여러 송신기가 동일한 차동 쌍을 공유하는 범용 멀티드롭 네트워크(general-purpose multidrop network)로 간주해서는 안 된다.

차동 전송(differential transmission)은 일반적인 RS-232보다 훨씬 긴 실용적인 통신 거리를 제공한다. RS-422 링크(link)는 적절한 저속 통신 조건에서 약 1,200미터에 이르는 케이블 길이에서도 동작할 수 있지만, 실제 가능한 거리는 보레이트(baud rate), 케이블 특성, 종단 처리(termination), 환경 잡음, 송수신기 성능(transceiver performance), 설치 품질에 크게 의존한다. 신호 속도가 증가하면 전파 지연(propagation delay), 감쇠(attenuation), 반사(reflection), 파형 왜곡(waveform distortion)의 영향이 증가하므로 일반적으로 허용 가능한 케이블 길이는 감소한다.

따라서 보레이트와 케이블 길이 사이의 관계는 RS-422 설계에서 핵심적인 고려 사항이다. 비교적 낮은 신호 속도에서는 각각의 신호 전이(signal transition)가 발생한 후 수신기가 안정된 차동 전압을 판별할 수 있는 시간이 충분하다. 반면 높은 속도에서는 각각의 비트가 차지하는 시간이 짧아지므로 케이블 전파 효과와 신호 안정화 시간이 사용 가능한 비트 주기(bit period)의 더 큰 부분을 차지하게 된다. 신뢰성 있는 설계를 위해서는 전송 거리와 신호 속도를 독립적으로 선택하지 않고 함께 평가해야 한다.

긴 케이블에서는 신호 전이 시간이 케이블의 전파 지연에 비해 충분히 짧아지면 케이블을 전송선(transmission line)으로 취급해야 한다. 케이블 임피던스(cable impedance)가 적절하게 제어되지 않으면 케이블 끝에 도달한 전기적 신호 전이가 송신기 방향으로 다시 반사될 수 있다. 이러한 반사는 차동 파형을 왜곡하고 여러 번의 임계값 교차(threshold crossing)를 발생시키거나 타이밍 여유(timing margin)를 감소시킬 수 있다. 케이블 길이와 신호 속도가 증가할수록 이러한 문제는 더욱 중요해진다.

RS-422 링크에서는 신호 반사를 감소시키기 위해 일반적으로 수신단에 종단 저항(termination resistance)을 배치한다. 종단 저항은 전송 케이블의 특성 임피던스(characteristic impedance)와 대략 일치하도록 선택하며, 일반적인 연선 통신 케이블에서는 약 100\~120옴(ohms) 정도가 사용되는 경우가 많다. 올바른 종단 처리는 케이블 끝에서 신호 에너지가 반사되는 대신 상당 부분 흡수되도록 하여 신호 전이를 깨끗하게 만들고 높은 속도 또는 장거리 통신에서 신뢰성을 향상시킨다.

그러나 종단 처리(termination)는 전체 전기 부하(electrical load)의 일부로 함께 고려해야 한다. 차동 쌍에 전압이 인가되는 동안 종단 저항에는 지속적으로 전류가 흐르므로 송신기는 케이블, 수신기 입력(receiver input), 종단 저항을 구동하면서 필요한 차동 전압을 유지할 수 있어야 한다. 잘못된 종단 저항값, 과도한 수신기 부하, 부적절한 케이블 또는 불필요한 추가 종단은 신호 진폭(signal amplitude)을 감소시키고 통신 신뢰성을 저하시킬 수 있다.

차동 신호 방식이 접지 차이에 대한 민감도를 감소시키기는 하지만 RS-422에서 접지(grounding)를 완전히 무시할 수 있는 것은 아니다. 송수신기 입력에는 제한된 공통 모드 전압 범위(common-mode voltage range)가 존재하며, 멀리 떨어진 장비 사이의 전위차가 지나치게 커지면 두 신호 도체 모두 허용 범위를 벗어날 수 있다. 따라서 서로 다른 전원 시스템에서 동작하거나 물리적으로 상당히 떨어져 있는 장비 사이에서는 기준 도체(reference conductor), 적절한 접지 전략, 갈바닉 절연(galvanic isolation), 절연형 송수신기(isolated transceiver)가 필요할 수 있다.

심각한 산업용 전자기 환경에서는 케이블 차폐(cable shielding)를 통해 추가적인 보호를 제공할 수 있다. 차폐층(shield)은 전기장 간섭(electric-field interference)을 차단하고 시스템의 접지 및 전자기 적합성 전략(EMC strategy)에 따라 적절하게 종단될 경우 불필요한 고주파 전류가 흐를 수 있는 제어된 경로를 제공한다. 그러나 차폐가 차동 신호, 올바른 연선 배선, 적절한 접지를 대체하는 것은 아니다. 신뢰성 높은 RS-422 설치를 위해서는 케이블 구조, 차폐, 접지, 종단, 커넥터 설계(connector design), 물리적 배선 경로를 함께 고려해야 한다.

RS-422는 완전한 응용 프로토콜(application protocol)이 아니라 전기적 신호 계층(electrical signaling layer)을 규정한다. 차동 인터페이스는 익숙한 시작 비트(start bit), 데이터 비트(data bit), 패리티(parity), 정지 비트(stop bit)를 사용하는 비동기 UART 데이터를 전달할 수 있지만 장비 설계에 따라 다른 직렬 신호 방식도 지원할 수 있다. 따라서 RS-422의 물리적 전기 인터페이스와 그 위에서 전달되는 프레이밍(framing) 및 응용 프로토콜을 구분해야 한다. 전압 인터페이스가 서로 일치한다는 사실만으로 프로토콜 호환성(protocol compatibility)이 보장되는 것은 아니다.

실제 RS-422 인터페이스는 일반적으로 디지털 컨트롤러(digital controller) 또는 UART와 RS-422 라인 드라이버(line driver) 및 수신기(receiver)로 구성된다. 송수신기(transceiver)는 논리 레벨의 TX 데이터를 케이블 전송에 적합한 상보적인 차동 신호로 변환하고, 수신된 차동 전압을 다시 논리 레벨의 RX 데이터로 변환한다. 이러한 구조를 통해 프로세서(processor), PLC, 임베디드 컨트롤러(embedded controller), 센서(sensor), 엔코더(encoder), 로봇 서브시스템(robot subsystem)은 내부적으로 일반적인 디지털 논리를 사용하면서 외부 통신에서는 차동 통신의 높은 전송 신뢰성을 활용할 수 있다.

RS-422 링크를 디버깅(debugging)할 때에는 각 도체를 개별적으로 확인하기보다 차동 쌍 전체를 분석해야 한다. 적절한 차동 측정 기능(differential measurement capability)을 갖춘 오실로스코프(oscilloscope)를 사용하면 차동 진폭, 극성, 신호 에지 품질(edge quality), 반사, 링잉(ringing), 공통 모드 변동(common-mode movement), 타이밍을 확인할 수 있다. 통신 오류가 발생하면 차동 쌍의 극성, 종단 위치, 케이블 연속성, 커넥터 핀 배열(connector pin assignment), 접지, 보레이트, 직렬 프레임 설정(serial frame configuration)도 함께 점검해야 한다.

RS-422는 비교적 단순한 직렬 통신 방식에 장거리 전송 능력, 높은 잡음 내성(noise immunity), 결정론적인 점대점 동작(deterministic point-to-point behavior)을 결합하기 때문에 산업 및 로봇 시스템에서 여전히 중요한 역할을 한다. 산업용 컨트롤러, 엔코더, 모션 시스템(motion system), 계측 장비(instrumentation), 레거시 자동화 장비(legacy automation equipment), 로봇 인터페이스 등에서 널리 사용된다. RS-422의 차동 신호 원리를 이해하는 것은 RS-485를 비롯하여 산업 통신 시스템 전반에서 사용되는 견고한 물리 계층 기술(physical-layer technology)을 이해하기 위한 중요한 기초가 된다.

## 01.03. Cable Length and Baud Rate

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

케이블 길이(cable length)와 보레이트(baud rate)는 직렬 통신(serial communication)에서 서로 밀접하게 연관된 파라미터(parameter)이다. 모든 물리적 케이블은 저항(resistance), 정전용량(capacitance), 인덕턴스(inductance), 전파 지연(propagation delay), 감쇠(attenuation), 전자기 간섭(electromagnetic interference)에 대한 민감성을 갖기 때문이다. 케이블이 길어질수록 이러한 영향으로 전송 파형이 더 크게 왜곡된다. 동시에 보레이트가 증가하면 각 심볼 주기(symbol period)가 짧아져 다음 신호 전이가 발생하기 전에 수신기가 유효한 전기적 상태를 판별할 수 있는 시간이 줄어든다.

보레이트(baud rate)는 초당 전송되는 신호 심볼(signaling symbol)의 수를 나타낸다. 일반적인 이진 비동기 직렬 통신(binary asynchronous serial communication)에서는 하나의 심볼이 보통 하나의 비트를 나타내므로 보레이트와 비트레이트(bit rate)가 동일한 수치로 표현되는 경우가 많다. 따라서 9600 보드 링크는 약 104.17 마이크로초(microseconds)의 공칭 비트 주기(nominal bit period)를 가지지만, 115200 보드 링크에서는 각 비트에 약 8.68 마이크로초만 사용할 수 있다. 따라서 높은 신호 속도는 물리적 연결에 훨씬 엄격한 전기적 및 타이밍 요구 조건을 부과한다.

케이블 정전용량(cable capacitance)은 비교적 긴 직렬 통신 링크(serial link)에서 특히 중요한 요소이다. 모든 케이블에는 도체 사이와 도체와 주변 차폐층(shield) 또는 접지 사이에 분포 정전용량(distributed capacitance)이 존재한다. 신호 상태가 변경될 때마다 송신기(transmitter)는 이 정전용량을 충전하고 방전해야 한다. 케이블 길이가 증가하면 전체 용량성 부하(capacitive load)가 증가하여 전압 전이가 느려지고 이상적인 디지털 파형에서 예상되는 급격한 전이 대신 둥글어진 신호 에지(signal edge)가 나타날 수 있다.

느린 신호 에지(slow signal edge)가 그 자체로 반드시 오류를 의미하는 것은 아니지만, 수신기가 명확한 논리 상태(logical state)를 관찰할 수 있는 시간을 감소시킨다. 낮은 보레이트에서는 케이블로 인해 상승 시간(rise time)과 하강 시간(fall time)이 상당히 증가하더라도 각 전이 이후 파형이 안정화될 수 있는 시간이 충분할 수 있다. 반면 높은 보레이트에서는 이전 신호가 완전히 안정화되기 전에 다음 비트 전이가 시작될 수 있어 잡음 여유(noise margin)가 감소하고 결국 잘못된 샘플링이나 프레이밍 오류(framing error)가 발생할 수 있다.

신호 감쇠(signal attenuation) 역시 전송 거리가 증가할수록 커진다. 케이블과 이에 연결된 전기적 부하는 급격한 파형 전이를 구성하는 고주파 성분(high-frequency component)의 진폭을 감소시킨다. 따라서 긴 케이블을 통과한 신호는 짧은 케이블의 경우보다 수신기에 도달하는 신호의 크기가 작고 파형도 더 많이 왜곡될 수 있다. 통신 신뢰성은 송신기 진폭, 수신기 감도(receiver sensitivity), 케이블 구조, 신호 방식, 보레이트, 종단 처리(termination), 주변 전자기 환경 등에 따라 결정된다.

전파 지연(propagation delay)은 케이블 길이와 신호 속도가 증가할수록 더욱 중요한 요소가 된다. 전기 신호는 케이블을 통해 순간적으로 전달되는 것이 아니라 유한한 속도로 이동한다. 일반적인 비동기 통신(asynchronous communication)에서는 수신된 문자가 특정한 전역 시간(global time)에 도착할 필요가 없기 때문에 절대적인 전파 지연보다 파형 무결성(waveform integrity)이 더 중요한 경우가 많다. 그러나 반사(reflection)와 신호 전이가 개별 비트의 지속 시간 안에서 서로 영향을 주기 시작하면 전파 지연도 중요한 설계 요소가 된다.

RS-232는 공통 신호 접지(common signal ground)를 기준으로 하는 단일 종단 신호 방식(single-ended signaling)을 사용하기 때문에 케이블 길이와 보레이트 사이의 관계에 특히 민감하다. 긴 케이블은 정전용량을 증가시키며 전자기 간섭에 노출되는 물리적 길이도 증가시킨다. 멀리 떨어진 장치 사이의 접지 전위차(ground potential difference) 역시 수신 전압을 교란할 수 있다. 따라서 전통적인 RS-232 설치는 RS-422나 RS-485와 같은 차동 표준(differential standard)에 비해 비교적 짧은 통신 거리와 연관된다.

흔히 언급되는 약 15미터의 RS-232 케이블 길이는 절대적인 물리적 한계라기보다는 전통적인 공학적 지침(engineering guideline)으로 이해해야 한다. 실제 가능한 통신 거리는 케이블 정전용량, 보레이트, 드라이버 특성(driver characteristics), 수신기 임계값(receiver threshold), 접지, 차폐(shielding), 커넥터 품질, 환경 잡음에 따라 달라진다. 낮은 속도의 RS-232 통신은 경우에 따라 이보다 상당히 긴 거리에서도 동작할 수 있지만, 전기적으로 열악한 환경에서는 중간 수준의 보레이트에서도 더 짧은 케이블이 필요할 수 있다.

RS-422는 연선(twisted pair)을 이용한 차동 신호 방식(differential signaling)을 사용하기 때문에 훨씬 긴 통신 거리를 제공한다. 수신기는 두 도체 사이의 전압 차이를 평가하므로 두 전선에 유사하게 결합된 잡음을 공통 모드 간섭(common-mode interference)으로 제거할 수 있다. 적절한 조건과 비교적 낮은 신호 속도에서는 RS-422 링크가 약 1,200미터에 이르는 거리에서도 동작할 수 있다. 이러한 특성으로 인해 RS-422는 긴 산업용 케이블 구간에서 RS-232보다 훨씬 적합하다.

그러나 RS-422의 최대 실용 통신 거리도 보레이트가 증가함에 따라 감소한다. 신호 전이 시간이 케이블 전파 지연에 비해 짧아지면 긴 케이블은 점차 전송선(transmission line)처럼 동작한다. 커넥터, 분기점(branch point), 케이블 끝에서 발생하는 임피던스 불연속(impedance discontinuity)은 신호 반사를 발생시킬 수 있다. 이러한 반사 신호가 새롭게 전송되는 신호 전이와 결합하면 차동 파형을 왜곡하여 중요한 샘플링 구간에서 수신되는 논리 상태를 불확실하게 만들 수 있다.

따라서 고속 또는 장거리 차동 통신에서는 종단 처리(termination)가 중요한 고려 사항이다. 수신단에 배치된 종단 저항(termination resistor)은 케이블의 특성 임피던스(characteristic impedance)와 대략 일치하도록 구성하여 도착한 신호 에너지를 흡수할 수 있다. 일반적인 연선 통신 케이블은 약 100\~120옴(ohms)의 특성 임피던스를 가질 수 있다. 올바른 종단 처리는 반사, 링잉(ringing), 반복적인 임계값 교차(threshold crossing)를 감소시켜 상당한 케이블 거리에서도 높은 보레이트의 통신 신뢰성을 향상시킨다.

케이블 구조(cable construction)는 구현 가능한 통신 거리와 속도의 조합에 직접적인 영향을 준다. 임피던스가 제어된 연선(controlled-impedance twisted pair)은 임의로 배치된 평행 전선보다 예측 가능한 전송 특성을 제공한다. 연선 구조는 두 도체를 외부 전자기장에 보다 균등하게 노출시켜 자기 및 전기적 간섭에 대한 내성을 향상시킨다. 낮은 정전용량의 케이블은 전기적 부하를 감소시키며, 적절한 차폐는 모터, 인버터(inverter), 스위칭 컨버터(switching converter), 릴레이 등 산업용 잡음원이 존재하는 환경에서 전자기 적합성(electromagnetic compatibility)을 향상시킬 수 있다.

커넥터(connector), 스플라이스(splice), 어댑터(adapter), 분기점 역시 신호 무결성(signal integrity)에 영향을 준다. 각각의 불연속 지점은 추가적인 정전용량, 인덕턴스, 접촉 저항(contact resistance), 임피던스 불일치(impedance mismatch)를 발생시킬 수 있다. 따라서 짧은 케이블을 사용하는 실험실 환경에서는 안정적으로 동작하던 통신 링크가 실제 로봇이나 공장에 설치된 이후 다르게 동작할 수 있다. 모터 상 케이블(motor phase cable), 대전류 배터리 배선, 인버터 출력, 스위칭 전력 변환기 근처에 통신 하네스(harness)를 배치하면 공칭 케이블 길이가 동일하더라도 통신 오류가 증가할 수 있다.

따라서 보레이트는 장치가 지원하는 가장 높은 값을 단순히 선택하기보다 응용 시스템이 실제로 요구하는 대역폭(bandwidth)에 따라 결정해야 한다. 설정 인터페이스(configuration interface), 진단 포트(diagnostic port), 온도 센서, 단순 컨트롤러, 저속 계측 장비는 비교적 적은 데이터 처리량만 필요로 하는 경우가 많다. 이러한 링크를 적절한 중간 수준의 보레이트로 동작시키면 응용 성능에 영향을 주지 않으면서 더 큰 타이밍 여유, 향상된 잡음 내성, 더 긴 케이블 통신 거리를 확보할 수 있다.

실제 응용 데이터 처리량(application throughput)은 직렬 프레이밍(serial framing)이 전송 용량을 사용하기 때문에 공칭 보레이트보다 낮다. 8N1 UART 설정에서는 8비트 데이터 바이트(byte) 하나를 전송하기 위해 시작 비트(start bit) 하나와 정지 비트(stop bit) 하나를 포함하여 총 10비트를 전송해야 한다. 따라서 115200 보드 연결의 이론적 최대 처리량은 프로토콜 헤더(protocol header), 체크섬(checksum), 응답(acknowledgment), 유휴 구간(idle interval), 재전송(retransmission), 소프트웨어 처리를 고려하기 전에 초당 약 11,520개의 8비트 문자이다.

따라서 신뢰성 있는 통신 설계에서는 먼저 필요한 페이로드 처리량(payload throughput)을 파악한 후 충분한 공학적 여유(engineering margin)를 확보할 수 있는 적절한 보레이트를 결정해야 한다. 이후 선택된 물리 인터페이스(physical interface)가 요구되는 케이블 거리와 실제 운용 환경에서 해당 속도를 지원할 수 있는지 확인해야 한다. RS-232가 충분한 통신 여유를 제공하지 못한다면 소프트웨어 재시도(retry)나 오류 처리만으로 신뢰성을 개선하려 하기보다 RS-422 또는 다른 차동 인터페이스로 변경하는 것이 더욱 효과적일 수 있다.

검증(verification)은 최종적으로 사용할 케이블 종류, 실제 설치와 유사한 길이, 커넥터, 종단 처리, 접지 구성, 대표적인 전자기 환경을 이용하여 수행해야 한다. 오실로스코프(oscilloscope) 측정을 통해 감소된 신호 진폭, 느린 상승 및 하강 시간, 링잉, 반사, 공통 모드 변동(common-mode movement), 불안정한 임계값 교차를 확인할 수 있다. 또한 이상적인 실험실 조건에서만 시험하지 않고 예상되는 온도, 공급 전압, 기계적 구성, 다양한 운전 모드에서 통신 오류 통계를 평가해야 한다.

산업용 로봇(industrial robot)과 자동화 시스템(automation system)에서 케이블 길이와 보레이트는 궁극적으로 데이터 처리량, 통신 거리, 잡음 내성(noise immunity), 배선 구조(wiring architecture), 신뢰성을 함께 고려하는 시스템 수준의 절충 관계(system-level trade-off)로 다루어야 한다. 짧은 로컬 연결(local connection)은 비교적 높은 신호 속도를 지원할 수 있지만, 긴 현장 연결(field connection)에서는 일반적으로 차동 신호 방식과 적절하게 낮춘 보레이트가 유리하다. 이러한 관계를 이해하는 것은 RS-232, RS-422, RS-485 및 이후의 산업 통신 기술을 적절하게 선택하기 위한 실용적인 기초가 된다.

## 01.04. Flow Control (HW/SW)

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

흐름 제어(flow control)는 직렬 통신(serial communication)에서 장치 사이에 데이터가 전송되는 속도를 조절하기 위해 사용하는 메커니즘이다. 송신기(transmitter)는 문자를 연속적으로 전송할 수 있지만, 수신 장치(receiving device)는 제한된 버퍼 용량(buffer capacity)을 가지고 있거나 수신된 정보를 처리하는 데 추가 시간이 필요할 수 있다. 흐름 제어가 없으면 수신기가 처리할 수 있는 범위를 초과하여 버퍼 오버플로(buffer overflow), 문자 손실, 메시지 손상 또는 프로토콜 오류(protocol failure)가 발생할 수 있으며, 이는 전기적 연결 자체가 정상적으로 동작하는 경우에도 발생할 수 있다.

흐름 제어가 필요한 이유는 통신 속도(communication speed)와 처리 속도(processing speed)가 반드시 동일하지 않기 때문이다. UART는 고정된 보레이트(baud rate)로 바이트(byte)를 수신하지만, 프로세서(processor)는 동시에 제어 알고리즘을 실행하거나 인터럽트(interrupt)를 처리하고 저장 장치에 접근하거나 다른 주변 장치와 통신해야 할 수 있다. 이러한 일시적인 처리 지연으로 수신 데이터가 버퍼에 누적될 수 있다. 흐름 제어는 버퍼가 가득 차기 전에 수신기가 송신 중단을 요청하고, 충분한 여유 공간이 확보되면 다시 전송하도록 한다.

직렬 통신에서는 일반적으로 하드웨어 흐름 제어(hardware flow control)와 소프트웨어 흐름 제어(software flow control)의 두 가지 방식이 사용된다. 하드웨어 흐름 제어는 TX와 RX 데이터 선 이외에 별도의 전기적 제어 신호(control signal)를 사용한다. 반면 소프트웨어 흐름 제어는 전송되는 데이터 스트림(data stream)에 특별한 제어 문자(control character)를 삽입한다. 두 방식 모두 동일한 기본 문제를 해결하지만 배선 요구 사항, 응답 특성, 프로토콜 투명성(protocol transparency), 적용 분야에서 상당한 차이가 있다.

RS-232의 하드웨어 흐름 제어는 일반적으로 RTS와 CTS 신호를 사용한다. RTS는 송신 요청(Request To Send)을 의미하며, CTS는 송신 가능(Clear To Send)을 의미한다. 일반적인 구현에서는 한 장치가 제어 출력을 통해 통신 진행 여부를 나타내고 다른 장치는 해당 입력 상태를 확인한 후 데이터를 전송한다. 다만 RTS와 CTS의 역사적인 의미와 실제 구현 방식은 장비마다 다를 수 있으므로, 엔지니어는 신호 이름만을 기준으로 판단하지 말고 연결되는 장치에서 RTS와 CTS가 실제로 어떻게 정의되어 있는지 확인해야 한다.

현대의 많은 UART 구현에서는 RTS와 CTS를 이용한 흐름 제어가 직렬 통신 하드웨어에 의해 자동으로 수행된다. 수신 버퍼(receiver buffer)가 설정된 임계값(threshold)에 가까워지면 UART는 흐름 제어 출력을 변경하여 원격 송신기(remote transmitter)에 전송 중지를 요청할 수 있다. 버퍼에 저장된 데이터가 충분히 처리되면 신호 상태가 다시 변경되어 전송이 재개된다. 이러한 메커니즘은 응용 소프트웨어와 독립적으로 동작할 수 있으므로 빠르게 반응하며 프로세서 스케줄링 지연(processor scheduling delay)으로 인한 데이터 손실 위험을 줄일 수 있다.

하드웨어 흐름 제어에는 추가적인 도체(conductor)와 양쪽 장치의 호환 가능한 인터페이스가 필요하다. 최소한의 비동기 직렬 연결(asynchronous serial connection)은 TX, RX, 기준 접지(reference ground)만으로 구성할 수 있지만, RTS/CTS 연결에는 추가적인 신호 배선이 필요하다. 전통적인 RS-232 시스템에는 DTR, DSR, DCD와 같은 모뎀 관련 제어 신호(modem-oriented control signal)도 포함될 수 있지만, 이러한 신호를 바이트 수준 흐름 제어(byte-level flow control)와 동일한 것으로 간주해서는 안 된다. 각각의 목적은 장비와 통신 구조에 따라 달라진다.

RTS/CTS 하드웨어 흐름 제어의 주요 장점은 사용자 데이터 스트림(user data stream)의 값을 소비하지 않는다는 것이다. 바이너리 패킷(binary packet)은 특정 바이트 값이 실수로 일시 정지 또는 재개 요청으로 해석될 위험 없이 임의의 바이트 값을 포함할 수 있다. 흐름 제어 상태는 별도의 전기적 신호선을 통해 독립적으로 전달된다. 이러한 특성으로 인해 하드웨어 흐름 제어는 바이너리 프로토콜(binary protocol), 연속 데이터 스트림, 높은 데이터 속도, 수신 버퍼 보호가 중요한 임베디드 시스템(embedded system)에 적합하다.

하드웨어 흐름 제어는 수신 UART가 버퍼 임계값에 도달하는 즉시 제어선 상태를 변경할 수 있기 때문에 비교적 빠른 응답을 제공한다. 그러나 흐름 제어 신호가 변경되었다고 해서 데이터 전송이 반드시 즉시 중단되는 것은 아니다. 이미 송신 UART, 선입선출 버퍼(FIFO), 드라이버(driver), 운영체제 버퍼(operating-system buffer), 통신 어댑터(communication adapter)에 문자가 존재할 수 있다. 따라서 수신기는 흐름 제어 상태가 변경된 이후에도 도착할 수 있는 데이터를 수용할 수 있도록 충분한 예비 버퍼 용량(reserve buffer capacity)을 확보해야 한다.

소프트웨어 흐름 제어는 일반 데이터와 동일한 직렬 데이터 경로를 통해 특별한 문자를 전송함으로써 별도의 제어 배선을 제거한다. 가장 대표적인 방식은 XON/XOFF 흐름 제어이다. XOFF는 관례적으로 제어 문자 DC3로 표현되며 일반적으로 16진수 값 0x13에 해당한다. XON은 제어 문자 DC1로 표현되며 일반적으로 16진수 값 0x11에 해당한다. 이러한 문자들은 원격 송신기에 데이터 전송을 일시 중지하거나 이후 다시 시작하도록 지시한다.

XON/XOFF를 사용하는 수신기가 입력 버퍼(input buffer)가 가득 차기 시작한다고 판단하면 XOFF 문자를 전송한다. 송신 장치는 이 문자를 흐름 제어 명령(flow-control command)으로 인식하여 일반 데이터의 전송을 일시적으로 중단한다. 수신기가 버퍼에 저장된 정보를 충분히 처리하면 XON을 전송하고 송신기는 다시 데이터 전송을 시작한다. 제어 정보가 일반적인 직렬 통신 채널(serial communication channel)을 통해 전달되므로 별도의 RTS 또는 CTS 도체가 필요하지 않다.

배선 요구 사항이 적다는 특성 때문에 소프트웨어 흐름 제어는 TX, RX, 접지만 사용할 수 있는 단순한 직렬 링크(serial link)에 유용하다. 과거에는 터미널(terminal), 모뎀(modem), 계측 장비(instrumentation), 기타 직렬 통신 장비에서 널리 사용되었다. 추가적인 하드웨어 핸드셰이크 선(hardware handshake line)을 사용할 수 없는 임베디드 또는 레거시 인터페이스(legacy interface)에서도 여전히 활용할 수 있다. 그러나 양쪽 장치는 동일한 소프트웨어 흐름 제어 동작을 명시적으로 지원하고 동일하게 설정해야 한다.

XON/XOFF의 주요 한계는 제어 문자가 일반 응용 데이터(application data)에 나타날 수도 있는 값을 사용한다는 것이다. 텍스트 중심의 통신에서는 일반적으로 관리할 수 있지만 임의의 바이너리 데이터에는 자연스럽게 0x11 또는 0x13 값이 포함될 수 있다. 통신 시스템이 이러한 값을 흐름 제어 명령으로 해석하면 데이터 전송이 예상하지 못하게 중지되거나 재개될 수 있다. 따라서 소프트웨어 흐름 제어가 활성화된 상태에서 바이너리 프로토콜을 사용하려면 이스케이프(escaping), 인코딩(encoding) 또는 다른 처리 메커니즘이 필요하다.

소프트웨어 흐름 제어는 일시 정지 명령 자체가 직렬 통신 경로를 통해 전달되고 송신기가 이를 수신하여 해석한 후에야 반응할 수 있기 때문에 하드웨어 흐름 제어보다 직접적인 응답이 느릴 수 있다. 버퍼와 대기 중인 문자(queued character)는 추가적인 지연을 발생시킬 수 있다. 하드웨어 흐름 제어와 마찬가지로 수신기는 버퍼가 완전히 가득 찰 때까지 기다렸다가 전송 중지를 요청해서는 안 된다. 이미 전송 중이거나 송신 대기 상태인 데이터를 수용할 수 있도록 충분한 여유 공간을 유지해야 한다.

흐름 제어는 오류 검출(error detection) 및 재전송(retransmission)과 구분해야 한다. RTS/CTS 또는 XON/XOFF는 수신기가 일시적으로 더 많은 데이터를 처리할 수 없어서 발생하는 데이터 손실을 방지하기 위한 것이지만, 전송된 비트가 전기적 잡음으로 손상되었는지를 판단하지는 않는다. 패리티(parity), 체크섬(checksum), 순환 중복 검사(cyclic redundancy check), 응답(acknowledgment), 시퀀스 번호(sequence number), 재전송 메커니즘은 서로 다른 통신 문제를 해결하며 프로토콜의 다른 계층에서 구현될 수 있다.

흐름 제어는 보레이트 일치(baud-rate matching)와도 다르다. 두 장치는 여전히 호환되는 보레이트, 데이터 비트 설정(data-bit setting), 패리티, 정지 비트 설정(stop-bit configuration)을 사용해야 한다. 흐름 제어는 잘못된 UART 타이밍이나 전기적 신호 문제를 보상하지 않는다. RTS/CTS가 완벽하게 동작하더라도 양쪽 장치의 보레이트가 크게 다르면 데이터가 손상될 수 있으며, 반대로 전기적 연결이 올바르게 구성되어 있더라도 수신 소프트웨어가 입력 문자를 충분히 빠르게 처리하지 못하면 데이터가 손실될 수 있다.

따라서 버퍼 설계(buffer design)는 흐름 제어 설계와 밀접하게 연관된다. UART 하드웨어에는 작은 선입선출 버퍼(FIFO)가 포함될 수 있으며, 장치 드라이버(device driver)와 운영체제(operating system)는 더 큰 소프트웨어 버퍼를 제공할 수 있다. 임베디드 응용 프로그램은 인터럽트 처리기(interrupt handler)와 처리 태스크(processing task) 사이에 추가적인 링 버퍼(ring buffer)를 구현할 수도 있다. 흐름 제어 임계값은 최종 버퍼 공간이 소진되기 전에 원격 송신기가 반응할 수 있는 충분한 시간과 공간을 제공해야 하므로 전체 버퍼 경로를 이해하는 것이 중요하다.

산업용 컨트롤러(industrial controller)와 로봇 시스템(robotic system)에서는 통신 동작과 시스템 제약 조건에 따라 적절한 흐름 제어 방식을 선택해야 한다. 낮은 데이터 속도의 설정 인터페이스(configuration interface)는 흐름 제어 없이도 안정적으로 동작할 수 있지만, 측정값이나 진단 정보를 지속적으로 전송하는 장치는 보다 강력한 버퍼 보호가 필요할 수 있다. 별도의 도체를 사용할 수 있다면 하드웨어 흐름 제어가 유리하며, 배선을 최소화해야 하고 전송 데이터 형식이 제어 문자를 안전하게 처리할 수 있다면 소프트웨어 흐름 제어를 선택할 수 있다.

문제 해결(troubleshooting) 과정에서는 설정과 물리적 배선을 모두 확인해야 한다. RTS/CTS를 사용하도록 설정된 시스템에서 잘못된 배선이나 원격 인터페이스의 미지원으로 CTS가 비활성 상태에 유지되면 데이터가 전혀 전송되지 않는 것처럼 보일 수 있다. 마찬가지로 XON/XOFF 링크에서는 바이너리 응용 데이터가 제어 문자로 해석되어 예상하지 못한 전송 중지가 발생할 수 있다. 따라서 직렬 터미널 설정(serial terminal setting), UART 구성, 커넥터 핀 배열(connector pin assignment), 케이블 배선, 프로토콜 문서를 함께 확인해야 한다.

적절하게 설계된 흐름 제어는 기본 보레이트를 변경하지 않으면서 통신 처리량(communication throughput)을 수신 장치의 상태에 따라 일시적으로 조절할 수 있도록 한다. 전송이 허용되는 동안 송신기는 설정된 신호 속도로 계속 동작하지만, 수신기가 추가적인 처리 시간을 필요로 하면 일시적으로 전송을 중지한다. 따라서 하드웨어 흐름 제어와 소프트웨어 흐름 제어를 모두 이해하는 것은 RS-232 및 관련 UART 기반 인터페이스(UART-based interface)를 산업 장비, 임베디드 컨트롤러, 센서, 로봇 통신 시스템에 통합하기 위한 중요한 기초가 된다.

## 01.05. RS232/RS422 in Robotics

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

RS-232와 RS-422는 많은 로봇 서브시스템(robot subsystem)이 고대역폭 네트워크(high-bandwidth network)보다 단순하고 결정론적이며 비용이 낮은 직렬 통신(serial communication)을 필요로 하기 때문에 로봇 시스템에서 여전히 유용하다. 센서(sensor), 모터 컨트롤러(motor controller), 엔코더(encoder), 위치 측정 장치(positioning device), 진단 인터페이스(diagnostic interface), 산업용 계측 장비(industrial instrument), 레거시 주변 장치(legacy peripheral)는 이러한 전기 표준 중 하나를 사용하는 UART 기반 인터페이스(UART-based interface)를 제공할 수 있다. 두 방식의 선택은 주로 통신 거리, 전자기 환경, 배선 구조, 필요한 데이터 속도, 장비 호환성에 따라 결정된다.

RS-232는 특히 로봇 내부의 짧은 점대점 연결(point-to-point connection)이나 외부 설정 및 서비스 인터페이스(configuration and service interface)에 적합하다. 임베디드 컴퓨터(embedded computer), 마이크로컨트롤러(microcontroller), 유지보수용 노트북은 적절한 RS-232 송수신기(transceiver)를 사용하여 간단한 TX와 RX 연결을 통해 주변 장치와 통신할 수 있다. 기술적으로 성숙하고 폭넓게 지원되기 때문에 시운전(commissioning), 파라미터 설정(parameter configuration), 펌웨어 진단(firmware diagnostics), 디버깅(debugging), 레거시 산업 장비와의 통신을 위한 실용적인 인터페이스를 제공한다.

일반적인 로봇용 RS-232 구조는 프로세서 UART와 전압 변환(voltage translation) 및 논리 반전(logical inversion)을 수행하는 RS-232 송수신기로 구성된다. 외부 케이블은 송수신기를 센서, 컨트롤러 또는 서비스 장치에 연결한다. 저전압 UART 핀과 RS-232 인터페이스는 전기적 레벨과 극성 규칙(polarity convention)이 서로 다르므로 직접 연결해서는 안 된다. 이러한 차이는 자체 제작한 로봇 컨트롤러 보드(robot controller board)를 상용 직렬 통신 장비와 통합할 때 특히 중요하다.

RS-232는 케이블이 로봇 내부의 전기적 잡음(electrical noise)이 많은 영역을 통과해야 하는 경우 상대적으로 불리해진다. 이동 로봇(mobile robot)에는 모터, 모터 드라이브(motor drive), DC-DC 컨버터(DC-DC converter), 배터리 스위칭 회로(battery switching circuit), 접촉기(contactor), 충전기(charger), 대전류 배선 등이 존재하며 상당한 전자기 간섭(electromagnetic interference)을 발생시킬 수 있다. RS-232는 접지를 기준으로 하는 단일 종단 신호 방식(single-ended signaling)을 사용하므로 특히 케이블이 길어지거나 전력 전자 장치 근처로 배선될 경우 잡음 결합(noise coupling)과 접지 전위차(ground-potential difference)로 인해 통신 여유가 감소할 수 있다.

RS-422는 차동 신호 쌍(differential signal pair)을 통해 정보를 전송함으로써 이러한 환경에서 더욱 강건한 물리 인터페이스(physical interface)를 제공한다. 두 도체에 유사하게 유도되는 잡음은 차동 수신기(differential receiver)에 의해 제거될 수 있으며, 연선 케이블(twisted-pair cable)은 전자기 간섭에 대한 내성을 더욱 향상시킨다. 따라서 RS-422는 메인 컨트롤러(main controller)에서 비교적 멀리 떨어진 엔코더, 측정 장치, 원격 센서(remote sensor), 모션 관련 장비(motion-related equipment), 산업용 주변 장치와 안정적으로 통신해야 할 때 유용하다.

전이중 RS-422 통신(full-duplex RS-422 communication)은 일반적으로 송신과 수신에 각각 하나씩 두 개의 차동 쌍을 사용한다. 독립적인 통신 경로를 사용하므로 방향 전환(direction switching) 없이 양방향으로 동시에 데이터를 전송할 수 있다. 이러한 동작은 명령을 수신하면서 측정 데이터를 지속적으로 반환하는 장치와의 통신을 단순화할 수 있다. 따라서 컨트롤러는 하나의 차동 채널로 설정 또는 제어 정보를 전송하면서 별도의 차동 채널을 통해 상태, 위치, 진단 정보 또는 센서 데이터를 동시에 수신할 수 있다.

로봇의 크기와 케이블 배선 경로(cable routing)는 인터페이스 선택에 큰 영향을 미친다. 소형 실내 로봇(compact indoor robot)은 통신 경로가 짧기 때문에 낮은 데이터 속도의 주변 장치 연결에 RS-232를 성공적으로 사용할 수 있다. 반면 대형 실외 자율이동로봇(outdoor AMR), 모바일 매니퓰레이터(mobile manipulator), 검사 플랫폼(inspection platform), 산업용 로봇은 액추에이터(actuator)와 전력 분배 장비(power distribution equipment) 주변을 통과하는 긴 하네스(harness)를 가질 수 있다. 이러한 시스템에서는 복잡한 이더넷 기반 통신 구조(Ethernet-based communication architecture)를 사용하지 않고도 RS-422를 통해 훨씬 큰 전기적 여유와 통신 거리를 확보할 수 있다.

RS-422를 사용하더라도 케이블 구조(cable construction)는 여전히 중요하다. 차동 쌍은 일반적으로 적절한 연선 케이블로 구성해야 하며, 신호 배선은 모터 상 배선(motor phase wire), 인버터 출력(inverter output), 대전류 배터리 케이블, 스위칭 전력 도체(switching power conductor)와 불필요하게 가까이 배치하지 않아야 한다. 심각한 전자기 환경에서는 차폐(shielding)가 유용할 수 있지만 그 효과는 올바른 접지와 종단 처리(termination)에 따라 달라진다. 차동 신호 방식은 강건성을 높여 주지만 근본적으로 잘못된 하네스 설계(harness design)를 보완할 수는 없다.

길거나 고속으로 동작하는 RS-422 링크는 케이블의 특성 임피던스(characteristic impedance)와 대략 일치하는 종단 처리(termination)가 필요할 수 있다. 적절한 종단 처리는 신호 전이를 왜곡하고 수신기 상태를 불확실하게 만들 수 있는 반사(reflection)와 링잉(ringing)을 감소시킨다. 이러한 영향은 보레이트(baud rate)와 케이블 길이가 증가할수록 더욱 중요해진다. 따라서 로봇 통신 설계에서는 송수신기 성능, 케이블 임피던스, 종단 처리, 커넥터 품질, 하네스 토폴로지(harness topology), 예상 신호 속도를 하나의 물리 계층 시스템(physical-layer system)으로 함께 고려해야 한다.

두 인터페이스 모두 접지(grounding)에 주의해야 한다. RS-232는 공통 신호 기준(common signal reference)에 직접 의존하기 때문에 과도한 접지 전위차에 특히 취약하다. RS-422는 공통 모드 외란(common-mode disturbance)에 더 높은 내성을 가지지만 송수신기에도 제한된 공통 모드 동작 범위(common-mode operating range)가 존재한다. 따라서 절연된 전원 도메인(isolated power domain), 긴 외부 케이블, 충전기, 고정형 산업 장비 또는 별도의 전원으로 동작하는 주변 장치를 포함하는 로봇에서는 신중하게 설계된 기준 연결이나 갈바닉 절연 직렬 송수신기(galvanically isolated serial transceiver)가 필요할 수 있다.

보레이트는 단순히 장치가 지원하는 최대값으로 설정하기보다 실제 로봇 데이터 요구 사항에 따라 선택해야 한다. 온도 센서, 설정 포트, 배터리 모니터(battery monitor), 진단 인터페이스는 비교적 낮은 데이터 처리량만 필요할 수 있다. 낮은 신호 속도는 특히 긴 케이블에서 더 큰 타이밍 여유(timing margin)와 신호 무결성 여유(signal-integrity margin)를 제공한다. 높은 데이터 속도가 필요한 장치는 페이로드 크기(payload size), 업데이트 주기(update frequency), 지연 시간 요구 사항(latency requirement), 케이블 길이, 설치 환경의 전기적 특성을 함께 평가해야 한다.

로봇 서브시스템이 컨트롤러가 일시적으로 처리할 수 있는 속도보다 빠르게 데이터를 생성하는 경우 흐름 제어(flow control)도 중요해질 수 있다. RS-232 장비는 양쪽 장치에서 지원되는 경우 RTS/CTS 하드웨어 흐름 제어(hardware flow control) 또는 XON/XOFF 소프트웨어 흐름 제어(software flow control)를 사용할 수 있다. 하드웨어 흐름 제어는 페이로드 내부의 특정 값을 예약하지 않으므로 바이너리 데이터 스트림(binary data stream)에 특히 유용하다. 그러나 많은 결정론적 센서 인터페이스(deterministic sensor interface)는 고정된 메시지 속도를 선택하고 수신 버퍼가 최악 조건의 트래픽을 처리하도록 설계함으로써 흐름 제어를 사용하지 않기도 한다.

직렬 통신은 그 위에서 전달되는 응용 프로토콜(application protocol)과 구분해야 한다. RS-232와 RS-422는 주로 전기적 신호 특성(electrical signaling characteristic)을 정의하지만, 로봇 장치는 해당 물리 인터페이스 위에서 ASCII 명령, 독자적인 바이너리 프레임(proprietary binary frame), 엔코더 메시지, 측정 패킷(measurement packet), 진단 기록(diagnostic record)을 교환할 수 있다. 따라서 장치를 통합할 때에는 전기적 호환성, 보레이트, 데이터 비트(data bits), 패리티(parity), 정지 비트(stop bits), 흐름 제어 설정, 패킷 구조(packet structure), 바이트 순서(byte order), 체크섬 동작(checksum behavior), 명령 의미(command semantics)를 모두 확인해야 한다.

직렬 인터페이스 상위에 프로토콜 수준의 오류 검출(error detection)을 추가하면 신뢰성을 더욱 향상시킬 수 있다. 로봇 센서 또는 컨트롤러는 체크섬(checksum)이나 순환 중복 검사(cyclic redundancy check)를 사용하여 손상된 메시지를 검출하고, 시퀀스 번호(sequence number)를 사용하여 누락된 패킷을 확인하며, 응답(acknowledgment)이나 타임아웃(timeout)을 이용해 통신 장애를 감지할 수 있다. 이러한 메커니즘은 RS-232 또는 RS-422의 물리적 강건성을 대체하는 것이 아니라 보완한다. 신뢰성 높은 로봇 인터페이스는 적절한 전기 설계와 적절한 프레이밍(framing) 및 통신 감시(communication supervision)를 함께 사용한다.

직렬 인터페이스는 소프트웨어 문제와 구분하기 어려운 서브시스템을 연결하는 경우가 많으므로 진단(diagnostics)이 특히 중요하다. 엔코더, 센서 또는 모터 컨트롤러가 데이터를 전송하지 않는 경우 잘못된 보레이트, 뒤바뀐 TX/RX 도체, 반대로 연결된 RS-422 차동 쌍 극성(differential pair polarity), 누락된 접지 기준, 잘못된 종단 처리, 커넥터 손상, 전자기 간섭 또는 응용 프로토콜 불일치가 원인일 수 있다. 계층별 문제 해결(layer-by-layer troubleshooting)을 사용하면 물리적 통신 장애와 상위 수준의 소프트웨어 문제를 효과적으로 분리할 수 있다.

따라서 오실로스코프(oscilloscope), 차동 프로브(differential probe), 논리 분석기(logic analyzer), USB-직렬 어댑터(USB-to-serial adapter), 터미널 소프트웨어(terminal software)는 로봇 개발 과정에서 유용한 도구가 될 수 있다. 측정을 통해 전압 레벨, 유휴 상태(idle state), 차동 극성, 비트 타이밍(bit timing), 신호 에지 품질(edge quality), 반사, 잡음을 확인할 수 있다. 물리적 파형을 먼저 확인한 후 UART 프레이밍을 분석하고 마지막으로 응용 패킷을 확인하는 방식은 직렬 통신 문제를 체계적으로 진단할 수 있는 방법을 제공한다.

RS-232와 RS-422는 현대적인 로봇 통신 구조(robot communication architecture) 내에서도 적절한 위치에 배치해야 한다. 이들은 비교적 단순한 주변 장치 및 서비스 링크(peripheral and service link)에 적합하지만, 다수의 분산 노드(distributed node)가 조정된 실시간 통신(real-time communication)을 필요로 하는 경우 CAN, CANopen, EtherCAT, 산업용 이더넷(industrial Ethernet) 또는 다른 네트워크를 대체할 수는 없다. 따라서 로봇은 이더넷이나 CAN을 주요 통신 백본(communication backbone)으로 사용하면서 특정 센서, 계측 장비, 유지보수 인터페이스 및 레거시 장치에 RS-232 또는 RS-422를 유지할 수 있다.

결과적으로 로봇 공학에서 RS-232와 RS-422의 공학적 가치(engineering value)는 기술적인 최신성보다는 실용적인 적합성(practical suitability)에 있다. RS-232는 짧은 로컬 연결(local connection)에 단순성과 높은 호환성을 제공하고, RS-422는 차동 신호 방식을 통해 직렬 통신을 더 긴 거리와 전기적으로 까다로운 환경까지 확장한다. 이들의 전기적 동작, 케이블 제한, 타이밍, 흐름 제어, 접지, 진단 방법을 이해하면 더 큰 로봇 시스템 내부에서 특수 목적 통신 링크(specialized communication link)로 안정적으로 통합할 수 있다.
