# 03-ROS2 and Data Distribution Service

# 1. ROS의 메시지 통신

- ROS에서는 프로그램의 재ㅐ사용성을 극대화하기 위하여 [노드(Node, 최소 단위의 실행 가능한 프로세서)](https://index.ros.org/doc/ros2/Tutorials/Understanding-ROS2-Nodes/) 단위의 프로그램을 작성하는데 이는 실행 가능한 하나의 프로그램과 같다.
- 하나 이상의 노드 또는 노드 실행을 위한 정보 등을 묶어 놓은 것을 패키지(package), 패키지 묶음을 메타패키지(metapackega)
- 메시지 통신으로 노드와 노드 사이에 입력과 출력 데이터를 주고 받는다. [메시지(message)](http://wiki.ros.org/Messages)는 integer, floating point, boolean, string 타입의 변수이며 메시지 안에 메시지를 품는 간단한 데이터 구조 및 메시지들의 배열 구조도 사용이 가능하다.

![03-ROS2%20and%20Data%20Distribution%20Service%20397e63a10048479c92b9d0ff083edf05/Untitled.png](03-ROS2%20and%20Data%20Distribution%20Service/Untitled.png)

---

---

# 2. ROS2와 DDS

- 메시지 통신 방법 [1. 토픽](https://index.ros.org/doc/ros2/Tutorials/Topics/Understanding-ROS2-Topics/) [2. 서비스](https://index.ros.org/doc/ros2/Tutorials/Services/Understanding-ROS2-Services/) [3. 액션](https://index.ros.org/doc/ros2/Tutorials/Understanding-ROS2-Actions/) [4.파라미터](https://index.ros.org/doc/ros2/Tutorials/Parameters/Understanding-ROS2-Parameters/)는 통신 방법의 목적과 사용 방법은 다르지만 토픽의 Publish와 Subscribe의 개념을 응용
- ROS에서는 자체 개발한 [TCPROS](http://wiki.ros.org/ROS/TCPROS) 통신 라이브러리를 사용하지만, ROS2에서는 [OMG(Object Management Group)](https://www.omg.org/)에 의해 표준화된 [DDS(Data Distribution Service)](https://www.omg.org/spec/DDS/)의 [DDSI-RTPS(Real-time publish subscribe)](https://www.omg.org/spec/DDSI-RTPS/About-DDSI-RTPS/)를 사용

![03-ROS2%20and%20Data%20Distribution%20Service/Untitled%201.png](03-ROS2%20and%20Data%20Distribution%20Service/Untitled%201.png)

- DDS 도입으로 [IDL(Interface Description Language)](https://en.wikipedia.org/wiki/Interface_description_language)를 사용하여 메시지 정의 및 직렬화를 더 쉽고 포괄적으로 다룰 수 있게 되었다.
- DCPS(data-centric publish-subscribe), DLRL(data local reconstruction layer)의 내용을 담아 재정한 통신 프로토콜인 DDSI-RTPS을 채용하여 실시간 데이터 전송을 보자아고, 임베디드 시스템에서도 사용할 수 있게 되었다.
- ROS Master없이 여러 DDS 프로그램 간 통신이 가능하고, [QoS(Quality of Service)](https://index.ros.org/doc/ros2/Concepts/About-Quality-of-Service-Settings/)를 매개변수 형태로 설정하여 TCP처럼 데이터 손실을 방지함으로써 신뢰도를 높이고, UDP처럼 통신 속도를 최우선시이 가능하며 [DDS-Security](https://www.omg.org/spec/DDS-SECURITY/About-DDS-SECURITY/) 도입으로 보안이 강화되었다.

![03-ROS2%20and%20Data%20Distribution%20Service%20397e63a10048479c92b9d0ff083edf05/Untitled%202.png](03-ROS2%20and%20Data%20Distribution%20Service/Untitled%202.png)

---

---

# 3. DDS란?

- DDS(Data Distribution Service)는 데이터를 중심으로 연결성을 갖는 미들웨어의 프로토콜(DDSI-RTPS)과 같은 DDS 사양을 만족하는 미들웨어 API
- 해당 미들웨어는 밑에 그림과 같이 ISO 7 계층 레이어에서 호스트 계층(Host layers)에 해당되는 4~7 계층에 해당되고 ROS2에서는 위 그림과 같이 운영 체제와 사용자 어플리케이션 사이에 있는 소프트웨어 계층. 이를 통해 시스템의 다양한 구성 요소를 쉽게 통신하고 데이터를 공유할 수 있게 된다.

![03-ROS2%20and%20Data%20Distribution%20Service/Untitled%203.png](03-ROS2%20and%20Data%20Distribution%20Service/Untitled%203.png)

---

---

# 4. DDS의 특징

## 4.1 산업 표준

- DDS는 분산 객체에 대한 기술 표준을 재정하기 위해 OMG(Object Management Group)이 관리하는 만큼 산업 표준으로 자리잡고 있다.
- OpenFMB, Adaptive AUTOSAR, MD PnP, GVA, NGVA, ROS2와 같은 시스템에서 DDS를 사용하며 산업의 기반이 되고 있다. 산업 표준을 지키고 있는 만큼 ROS2가 IoT, 자동차, 국방, 항공, 우주 분야로 넓혀갈 수 있는 발판이 마련되었다.

---

## 4.2 운영체제 독립

- DDS는 Linux, Windows, macOS, Android, VxWorks 등 다영한 운영체제를 지원하고 있기에 사용자가 사용하던 운영체제에서 사용할 수 있다.

---

## 4.3 언어 독립

- ROS2에서 DDs를 RMW(ROS Middleware)으로 디자인하고 벤더별로 각 RMW가 제작되었으며 rclcpp, rclc, rclpy, rcljava, rclobjc, rclada, rclgo, rclnodejs같이 다양한 언어를 지원하는 ROS Client Library로 멀티 프로그래밍 언어를 지원하고 있다.

![03-ROS2%20and%20Data%20Distribution%20Service/Untitled%204.png](03-ROS2%20and%20Data%20Distribution%20Service/Untitled%204.png)

---

## 4.4 UDP 기반의 전송 방식

- UDP 기반의 신뢰성 있는 멀티캐스트(Multicast)를 구현하여 시스템이 최신 네트워킹 인프라의 이점을 효율적으로 활용 가능하다.
- UDP의 멀티캐스트는 브로드캐스트(broadcast)처럼 여러 목적지로 동시에 데이터 전송이 가능하지만, 불특정 목적지가 아닌 특정 도메인 그룹에 대해서만 전송이 가능하다.
- 이 멀티캐스트의 도입으로 ROS2에서는 DDS Global Space라는 공간에 있는 토픽들에 대해 구독 및 발행이 가능하다.

---

## 4.5 데이터 중심적 기능

- DCPS(data-centric publish-subscribe)는 적절한 수신자에게 적절한 정보를 효율적으로 전달하는 것을 목표로 하는 발간 및 구독 방식

![03-ROS2%20and%20Data%20Distribution%20Service/Untitled%205.png](03-ROS2%20and%20Data%20Distribution%20Service/Untitled%205.png)

---

## 4.6 동적 검색

- DDS의 동적 검색(Dynamic Discovery)을 통하여 어떤 토픽이 지정 도메인 영역에 있으며 어떤 노드가 이를 발신하고 수신하는지 알 수 있게 된다. ROS 프로그래밍할 때 데이터를 주고 받을 노드들의 IP 주소 및 포트를 미리 입력하거나 따로 구성하지 않아도 되며, 사용하는 시스템 아키텍처의 차이점을 고려할 필요가 없어 모든 플렛폼에서 쉽게 작업이 가능하다.
- ROS1의 ROS Master에서는 ROS 시스템의 노드들의 이름 지정 및 등록 서비스를 제공하고, 각 노드에서 pub&sub하는 메시지를 찾아 연결하는 정보를 제공하면서 노드들의 정보를 관리하여 서로 연결해야하는 노드들에게 서로의 정보를 건네주는 중요한 역할을 수행했었다.
- 반면, ROS2에서는 DDS의 동적 검색 기능을 사용하여 노드를 DDS의 Partucipant 개념으로 취급하고, DDS 미들웨어를 통해 직접 검색하여 노드 연결이 가능하다.

---

## 4.7 확장 가능한 아키텍처

- DDS의 Participant 형태의 노드는 확장 가능한 형태로 제공되어 사용 가능하고 단일 표준 통신 계층에서 많은 복잡성을 흡수하여 분산 시스템 개발을 단순화시켜 편의성을 높혔다.
- 이는 최소 실행 가능한 노드 단위로 나누어 수천 개의 노드를 관리하는 시스템에서 이 부분이 강점이며 복수의 로봇, 주변 인프라와 다양한 IT 기술, 데이터베이스, 클라우드로 연결 및 확하는 ROS시스템에 매우 적합한 기능이다.

---

## 4.8 상호 운영성

- DDS는 상호 운영성을 지원하는데 DDS의 표준 사양을 지키는 벤더 제품을 사용한다면 다른 회사의 제품으로 변경하거나 혼용하여 사용해도 제품 간 상호 통신이 지원한다.
- Fast DDS와 Cyclone DDS는 오픈 소스를 지향하기에 자유롭게 사용 가능하며 고성능을 원하면 OpenSplice, Connext DDS, Gurum DDS를 사용하면 된다.

![03-ROS2%20and%20Data%20Distribution%20Service/Untitled%206.png](03-ROS2%20and%20Data%20Distribution%20Service/Untitled%206.png)

---

## 4.9 서비스 품질(QoS)

- ROS2에서 QoS는 TCP처럼 데이터 손실을 방지함으로써 신뢰도를 우선시하거나, UDP처럼 통신 속도를 우선시하여 사용할 수 있게 하는 신뢰성 기능이 대표적이다.
- 통신 상태에 따라 정해진 사이즈만큼 데이터를 보관하는 History 기능, Subscriberk 생성되기 전의 데이터를 사용 혹은 폐기하는 Durability 기능
- 정해진 주기 안에 데이터가 발신 및 수신되지 않으면 이벤트 함수를 실행시키는 Deadline 기능
- 정해진 주기 안에서 수신되는 데이터만 유효 판정하고 그렇지 않은 데이터는 삭제하는 Lifespan 기능
- 정해진 주기 안에서 노드 혹은 토픽의 새사를 확인하는 Live liness
- 다양한 Qos 설정을 통해 DDS는 적시성, 트래픽 우선순위, 안정성 및 리소스 사용과 같은 데이터를 주고받는 모든 측면을 사용자가 제어할 수 있게 되었고 특정 상황에서 데이터 송/수신에 다양한 옵션 설정으로 문제 해결 및 으용이 가능하다.

---

## 4.10 보안

- DDS-Security 보안 사양으로 ROS1의 보안 단점을 해결하였다.
- ROS 커뮤니티에서 [SROS2(Secure Robot Operating System 2)](https://github.com/ros2/sros2) 툴을 개발하여 로보틱스 개발자를 위해 보안을 위한 툴킷을 배포하고 있다.

---

---

# 5. ROS에서의 사용법

## 5.1 기본적인 퍼블리셔 노드와 서브스크라이브 노드 실행

- ros2 run demo_nodes_cpp listener와 ros2 run demo_nodes-coo talker 명령어를 각 터미널 창에서 실행하면 listener 노드와 talk 노드가 실행된다. 이는 RMW(ROS middleware)를 사용하며 rqt_graph를 실행하면 아래 그림과 같이 실행된 Publisher Node, Subscriber Node를 확인할 수 있다.

```bash
$ ros2 run demo_nodes_cpp listener
[INFO]: I heard: [Hello World: 1]
[INFO]: I heard: [Hello World: 2]
[INFO]: I heard: [Hello World: 3]
[INFO]: I heard: [Hello World: 4]
[INFO]: I heard: [Hello World: 5]
```

```bash
$ ros2 run demo_nodes_cpp talker
[INFO]: Publishing: 'Hello World: 1'
[INFO]: Publishing: 'Hello World: 2'
[INFO]: Publishing: 'Hello World: 3'
[INFO]: Publishing: 'Hello World: 4'
[INFO]: Publishing: 'Hello World: 5'
```

```bash
$ rqt_graph
```

![03-ROS2%20and%20Data%20Distribution%20Service%20397e63a10048479c92b9d0ff083edf05/Untitled%207.png](03-ROS2%20and%20Data%20Distribution%20Service%20397e63a10048479c92b9d0ff083edf05/Untitled%207.png)

---

## 5.2 RMW 변경 방법

- RMW를 변경하여 사용하려면 ROS2를 지원하는 RMW 중 하나를 선택하여 'RMW_IMPLEMENTATION'이라는 환경 변수로 선언한 후 노드를 실행하면 된다.

```bash
- rmw_connext_cpp

- rmw_cyclonedds_cpp

- rmw_fastrtps_cpp

- rmw_gurumdds_cpp

- rmw_opensplice_cpp
```

```bash
$ export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
$ ros2 run demo_nodes_cpp listener
[INFO]: I heard: [Hello World: 1]
[INFO]: I heard: [Hello World: 2]
[INFO]: I heard: [Hello World: 3]
```

```bash
$ export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
$ ros2 run demo_nodes_cpp talker
[INFO]: Publishing: 'Hello World: 1'
[INFO]: Publishing: 'Hello World: 2'
[INFO]: Publishing: 'Hello World: 3'
```

---

## 5.3 RMW의 상호 운용성 테스트

- RMW 변경해서 DDS의 특징 '4.8 상호 운용성'을 테스트하고자 한다면 다음과 같이 ROS2를 지원하는 RMW 중 하나를 선택해 각 노드 별로 'RMW_IMPLEMENTATION' 환경 변수로 선언하면 된다.
- 'rmw_cyclonedde_cpp'과 'rmw_fastrtps_cpp'를 테스트하여 listener 노드와 talker 노드가 서로 다른 RMW를 사용해도 문제없이 통신되도록 해보자

```bash
$ export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
$ ros2 run demo_nodes_cpp listener
[INFO]: I heard: [Hello World: 1]
[INFO]: I heard: [Hello World: 2]
[INFO]: I heard: [Hello World: 3]
```

```bash
$ export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
$ ros2 run demo_nodes_cpp talker
[INFO]: Publishing: 'Hello World: 1'
[INFO]: Publishing: 'Hello World: 2'
[INFO]: Publishing: 'Hello World: 3'
```

---

## 5.4 Domain 변경 방법

- ROS2에서는 '4.4 UDP 기반의 전송 방식'에서 UDP 멀티캐스트로 통신이 이루어지기 때문에 별도의 설정을 하지 않으면 네트워크의 모든 노드가 연결된다. 이를 방지하려면 다른 네트워크를 사용하거나 'name space'를 변경하면 되며 가장 간단한 방법으로는 DDS의 'domain' 변경이 가장 간단하다.
- ROS의 RMW에서는 이를 'ROS-DOMAIN_ID'라 하며 횐경 변수로 설정이 가능하다.

```bash
$ export ROS_DOMAIN_ID=11
$ ros2 run demo_nodes_cpp talker
[INFO]: Publishing: 'Hello World: 1'
[INFO]: Publishing: 'Hello World: 2'
[INFO]: Publishing: 'Hello World: 
```

```bash
$ export ROS_DOMAIN_ID=12
$ ros2 run demo_nodes_cpp listener
```

```bash
$ export ROS_DOMAIN_ID=11
$ ros2 run demo_nodes_cpp listener
[INFO]: I heard: [Hello World: 13]
[INFO]: I heard: [Hello World: 14]
[INFO]: I heard: [Hello World: 15]
```

- 위의 사항으로 'ROS_DOMAIN_ID'을 서로 동일하게 맞춘 talker node와 listener node만이 서로 통신 됨을 알 수 있다.

---

## 5.5 QoS 테스트

- 4.9 서비스 품질(QoS)'에서 설명한 'TCP처럼 데이터 손실을 방지하여 신뢰도를 우선시하거나, UDP처럼 통신 속도를 우선시하여 사용할 수 있게 하는 신뢰성 기능'을 위한 간단한 테스트를 해보자

## 5.5.1 Reliability가 RELIABLE로 되어 있을 때

- demo_nodes_cpp 패키지의 listener_best_effort node를 사용하여 테스트해보자

```bash
$ sudo tc qdisc add dev lo root netem loss 10%
```

- 위의 명령어는 tc로 설정한 데이터의 손실을 주는 명령어이다.

```bash
$ ros2 run demo_nodes_cpp listener_best_effort
[INFO]: I heard: [Hello World: 1]
[INFO]: I heard: [Hello World: 3]
[INFO]: I heard: [Hello World: 4]
[INFO]: I heard: [Hello World: 5]
[INFO]: I heard: [Hello World: 6]
[INFO]: I heard: [Hello World: 7]
[INFO]: I heard: [Hello World: 8]
[INFO]: I heard: [Hello World: 10]
[INFO]: I heard: [Hello World: 11]
[INFO]: I heard: [Hello World: 12]
[INFO]: I heard: [Hello World: 13]
[INFO]: I heard: [Hello World: 14]
[INFO]: I heard: [Hello World: 15]
```

```bash
$ ros2 run demo_nodes_cpp talker
[INFO]: Publishing: 'Hello World: 1'
[INFO]: Publishing: 'Hello World: 2'
[INFO]: Publishing: 'Hello World: 3'
[INFO]: Publishing: 'Hello World: 4'
[INFO]: Publishing: 'Hello World: 5'
[INFO]: Publishing: 'Hello World: 6'
[INFO]: Publishing: 'Hello World: 7'
[INFO]: Publishing: 'Hello World: 8'
[INFO]: Publishing: 'Hello World: 9'
[INFO]: Publishing: 'Hello World: 10'
[INFO]: Publishing: 'Hello World: 11'
[INFO]: Publishing: 'Hello World: 12'
[INFO]: Publishing: 'Hello World: 13'
[INFO]: Publishing: 'Hello World: 14'
[INFO]: Publishing: 'Hello World: 15'
```

- talker node는 1부터 15까지 데이터를 전송했지만 listener_best_effort node는 데이터 손실로 인하여 2와 9가 손실된 채 표시된다.
- 신뢰성 중심이 아닌 속도 중심이고 데이터 손실이 있어도 문제없는 데이터라면 Reliability를 BEST_EFFORT로 설정하여 사용하면 된다.

```bash
$ sudo tc qdisc delete dev lo root netem loss 10%
```

- 위의 명령어로 임의로 설정한 데이터 손실을 해제하는 명령어이다.