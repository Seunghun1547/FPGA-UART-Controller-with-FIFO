# 📡 UART 기반 다기능 센서 제어 및 모니터링 시스템
> **Basys3 FPGA를 활용한 초음파/온습도 센서 제어, 스톱워치 구현 및 UART 통신 기반 데이터 모니터링 시스템**

본 프로젝트는 FPGA를 메인 컨트롤러로 사용하여 초음파(SR04), 온습도(DHT11) 센서를 제어하고, 처리된 데이터를 7-Segment와 PC(UART)로 실시간 출력하는 통합 임베디드 시스템 설계 프로젝트입니다.

---

## 1. 프로젝트 개요 (Introduction)
* **목적**: 하드웨어 기반의 센서 제어 로직 설계 및 시리얼 통신을 통한 데이터 가공/전송 프로세스 정립.
* **핵심 기능**:
    * **Command Control Unit**: PC로부터 수신된 명령에 따라 동작 모드(Stopwatch, Watch, SR04, DHT11) 및 트리거 제어.
    * **Sender & FIFO**: 센서로부터 수집된 데이터를 UART 규격에 맞춰 가공하고 FIFO 버퍼를 통해 안정적으로 PC에 전송.
    * **Multi-Display**: 측정된 데이터를 보드의 7-Segment, LED 및 PC 터미널에 동시 출력.

---

## 2. 기술 스택 (Tech Stack)
* **Language**: Verilog HDL
* **Hardware**: Basys3 FPGA, HC-SR04(초음파), DHT11(온습도)
* **Interface**: UART, I/O Port (Trig/Echo, 1-Wire)
* **Tools**: Vivado

---

## 3. 시스템 아키텍처 (System Architecture)
* **Top Module**: 전체 서브 모듈(Stopwatch, Watch, SR04, DHT11, Command Unit, Sender, FND Controller) 통합 및 신호 라우팅.
* **Peripheral Interface**:
    * **SR04**: Trig 신호 발생 및 Echo 펄스 폭 측정을 통한 거리 환산 로직 설계.
    * **DHT11**: 1-Wire 통신 프로토콜 기반의 습도/온도 데이터 추출 및 타이밍 제어.

---

## 🛠️ 4. Trouble Shooting (핵심 문제 해결 경험)

### 💥 Watch 모듈 초기화 오류 (Reset Logic)
* **문제 상황**: 시스템 리셋 시 Watch(시계) 모듈의 시간 값이 불규칙하게 초기화되는 현상 발생.
* **원인 분석**: 비동기 리셋 신호의 전파 지연으로 인해 카운터 레지스터들이 서로 다른 시점에 초기화되는 타이밍 이슈 확인.
* **해결 방법**: 동기 리셋(Synchronous Reset) 방식으로 구조를 변경하고, 리셋 신호를 클럭에 동기화시켜 모든 레지스터가 동일한 에지에서 동작하도록 수정.

### 💥 UART 데이터 전송 병목 및 깨짐 현상
* **문제 상황**: 다량의 센서 데이터를 연속 전송할 때 데이터가 누락되거나 문자가 깨져서 출력됨.
* **원인 분석**: UART 전송 속도 대비 데이터 생성 속도가 빨라 발생한 Overrun 및 핸드셰이킹 부재.
* **해결 방법**: **FIFO(First-In-First-Out) 버퍼**를 설계하여 데이터를 임시 저장하고, UART의 Ready/Busy 상태를 체크하여 전송 가능 시점에만 데이터를 송신하는 로직 적용.

---

## 5. 결과 및 성과
* **다양한 프로토콜 숙련**: UART, 1-Wire, PWM(Trig) 등 다양한 하드웨어 인터페이스 제어 경험 확보.
* **모듈형 설계 능력**: 각 기능을 독립적인 모듈로 설계하고 TOP 모듈에서 통합하는 계층적 설계(Hierarchical Design) 방식 습득.
* **실시간 데이터 처리**: 센서의 아날로그성 데이터를 디지털 수치로 정교하게 변환하고 전송하는 전과정 수행.
