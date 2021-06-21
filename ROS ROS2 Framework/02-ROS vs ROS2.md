# 02-ROS vs ROS2

[ROS vs ROS2](02-ROS%20vs%20ROS2%205bc9f5d7ec2648d9876afe95ecc473c6/ROS%20vs%20ROS2%20049647e06aa54d77a7bd04bb5c2f3fa0.csv)

---

---

## ROS2의 특징

1. Platforms
    - ROS2부터 Linux, macOS, windows 모두 지원
    - Microsoft가 ROS2 TSC로 들어와 windows 사용자가 ROS2에 쉽게 접근([링크1](https://microsoft.github.io/Win-RoS-Landing-Page/), [링크2](https://ms-iot.github.io/ROSOnWindows/))
2. Real-time
    - ROS2부터 [Real-time](http://design.ros2.org/articles/realtime_background.html)을 지원. 단, 특정 하드웨어, 운영체제 사용, DDS의 [RTPS](https://www.omg.org/spec/DDSI-RTPS/About-DDSI-RTPS/)같은 통신 프로토콜을 사용
3. Security
    - ROS1에서 ROS master의 하나의 Ip와 포트만 노출되면 모든 시스템 다운이 가능. TCPROS의 단점
    - ROS2부터 DDS도입하면서 DDS-Security로 보안에 대한 이슈를 통신단부터 해결
    - [SROS2(Secure Robot Operating System 2)](https://github.com/ros2/sros2)라는 툴을 개발하여 RCL 서ㅗ트 및 보안 관련 프로그래밍에 익숙하지 않은 개발자를 위해 툴킷 배포
4. Communication
    - ROS2는 [RTPS(Real time pub sub)](https://en.wikipedia.org/wiki/Data_Distribution_Service)를 지원하는 통신 미들웨어 DDS 사용
    - DDS에서 [IDL(interface Description Language)](https://en.wikipedia.org/wiki/Interface_description_language)를 사용하여 더 쉽고 포괄적인 메시지 정의 및 직렬화 가능
    - RTPS로 실시간 데이터 전송 보장 및 임베디드 시스템에 적용 가능
    - ROS Master가 없어도 여러 DDS 프로그램 간 통신 가능
    - [QoS(Quality of Service)](https://index.ros.org/doc/ros2/Concepts/About-Quality-of-Service-Settings/)로 신뢰도를 높이거나, subscribe형 메시지 전달, 실시간 데이터 전송, 불안정한 네트워트 대응, 보안 강화
5. Middleware interface
    - ROS2에서 벤더들의 미들웨어를 유저가 원하는 사용 목적에 맞게 선택하여 사용할 수 있도록 [ROS Middleware(RMW)](http://design.ros2.org/articles/ros_middleware_interface.html) 형태로 지원
6. Node manager(discovery)
    - ROS2에서 roscore가 없어지고 ROS Parameter Server, rosout logging node가 각각 독립으로 수행
    - ROS Master가 삭제되고 노드를 [DDS의 Participant](http://design.ros2.org/articles/Node_to_Participant_mapping.html)로 취급하고, [Dynamic Discovery](http://design.ros2.org/articles/discovery_and_negotiation.html) 기능을 이용하여 DDS  Middleware로 직접 검색하여 노드 연결이 가능
7. Languages
    - ROS2 Foxy 기준으로 모던 C++(C++14)과 Python 3.7 이상 프로그래밍 언어 지원
8. Bulid system
    - [ament(ROS1의 catkit 업그레이드 버전)](http://design.ros2.org/articles/ament.html)를 사용하여 Python 패키지 관리가 가능하여 완전 독립 실현
    - ROS2에서 Python 패키지는 setup.py 파일의 모든 기능을 순수 Python 모듈과 동등한 수준으로 개발 가능
9. Build tools
    - ROS1에서 catkin_make, catkin_make_isolated, catkin_tools 지원
    - ROS2에서는 ROS2 패키지를 작성, 테스트, 빌드 등 ROS2 기반의 프로그램 할 때 필수 툴로 작업 흐름을 향상시키는 CLI 타입의 명령어 도구로 [colcon](https://colcon.readthedocs.io/en/released/index.html) 사용
10. Build options
    - Multiple workspace - ROS2에서 복수의 독립된 워크스페이스를 사용할 수 있어서 작업 목적 및 패키지를 종류별로 관리가 가능
    - No non-isolated build - ROS에서 하나의 CMake 파일로 여려 개의 패키지를 빌드하면 속도가 빨라지지만 패키지의 종속성과 빌드 순서가 중요하며 충돌이 발생할 수 있다. 반면, ROS2에서는 격리된 빌드를 지원함으로써 모든 패키지를 별도로 빌드함
    - No devel space - catkin은 패키지를 빌드한 후 devel이라는 폴더에 코드를 저장하는데 패키지를 설치할 필요가 없이 사용할 수 있는 환경을 제공함. 편리한 기능이지만 패키지 관리 측면에서 복잡함. 반면 ROS2에서는 빌드한 후 패키지를 설치해야 함. colcon으로 심벌릭 링크 설치의 선택적 기능을 사용하여 동일한 이점을 제공
11. Version control system
    - ROS2에서 vcstool로 통합되면서 ROS에서도 vcstool을 사용
    - vcstool은 여러 리포지토리 작업을 쉽게 관리하는 버전 관리 시스템 툴
12. Client library

    ROS에서는 user land → ROS Client Library → Middleware interface 방식과 roscpp, rospy, roslisp 등 client libarary 제공.

    ROS2에서는 RCL(ROS Client Library) 이름과 rclcpp, rclc, rclpy, rclpy, rcljava, rclobjc, rclada, rclgo, rclnodejs 프로그래밍 언어로 제공.

13. Lift cycle
    - ROS에서 상태천이 제어 기능을 위해 [SMACH](http://wiki.ros.org/smach)같은 독립적인 패키지를 사용하고, Client Library에서는 사용자가 임의로 Client Library 부분까지 수정하여 사용
    - ROS2에서는 패키지의 각 노드들의 현 상태를 모니터링하고 상태를 제어할 수 있는 [life cycle](http://design.ros2.org/articles/node_lifecycle.html)을 Client Library에 추가함. 이로써 노드의 상태를 모니터링하고 상태를 천이시키거나 노드를 상태에 따라 재시작 또는 교체할 수 있어서 시스템 상태를 효과적으로 제어가 가능
14. Multiple nodes
    - ROS1에서 nodelet 기능으로 하드웨어 리소스가 제한적이거나 노드 간 수 많은 메시지를 보낼 때 사용
    - ROS2에서는 components를 사용하여 노드의 실행 파일 수준은 더 세분화 시키고, 프로세스 내 통신 기능을 이용하여 ROS2의 통신 오버 헤드를 제거할 수 있어 효율적인 ROS2 응용 프로그램 작성이 가능
15. Threading model
    - ROS1에서 단일 스레드 또는 다중 스레드 실행만 가능
    - ROS2에서는 C++과 Python으로 더 세분화 된 실행 모델을 사용하며 사용자가 정의한 실행기도 제공되는 RCL API를 이용하여 구현 가능
16. Messages(topic, service, action)
    - 단일 데이터 구조인 메시지로 각 패키지 이름과 지정된 형식으로 메시지를 고유하게 식별 가능
    - ROS2에서는 OMG(Object Management Group)에서 정의된 [IDL(Interface Description Language)](http://design.ros2.org/articles/idl_interface_definition.html)을 사용해 메시지 정의 및 직렬화가 더 쉽고 포괄적으로 다룰 수 있다.
17. Command Line Interface
    - CLI 명령어 사용법은 ROS1과 ROS2가 매우 비슷해서 이름 변경과 일부 옵션 사용법만 익히면 큰 어려움은 없음.
18. roslaunch
    - ROS에서 'run'은 단일 프로그램 실행, 'launch'는 사용자 지정 프로그램 실행. 'launch'는 사용자가 실행하고자 하는 프로그램의 각종 설정을 기술하고 기술된 설정에 맞추어 각종 프로그램 실행을 도와줌
    - 다양한 파일을 사용하는 점이 [ROS1과 ROS2의 차이점](http://design.ros2.org/articles/roslaunch.html)
    - ROS1에서 'launch'파일이 특정 'XML' 형식을 사용하여 다양한 설정을 추가하여 프로그램 실행이 가능
    - ROS2에서는 'XML'외에도 Python이 새롭게 채용되어 조건문과 Python 모듈을 사용하여 복잡한 논리와 기능 사용이 가능
19. Graph API
    - ROS의 '[rqt_graph](http://wiki.ros.org/rqt_graph)'는 각 노드와 토픽, 메시지 등 고유 이름으로 매핑이 이루어져 각 노드와 노드 간의 토픽, 메시지 관계를 그래프화 가능
    - ROS2에서는 노드 실행 도중에 다시 매핑이 가능하고, 바로 [그래프로 표현](https://index.ros.org/doc/ros2/Concepts/)이 가능할 예정
20. Embedded Systems
    - ROS에서 매우 기초적인 수단으로 임베디드 보드와 메시지를 주고 받을 때 시리얼(rosserial)로 통신
    - ROS2에서는 시리얼 통신, 블루투스 및 와이파이 통신, RTOS 사용이 가능하고, DDS-XRCE(eXtremely Resource Constrained Environments)를 사용하는 등 임베디드 보드에서 직접 ROS 프로그래밍을 하여 하드웨어 펌웨어로 구현된 노드를 실행 가능