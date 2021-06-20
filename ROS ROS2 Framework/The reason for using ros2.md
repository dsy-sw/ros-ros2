# The reason for using ros2

- [**ROS1 마지막 버전 선언**](https://www.openrobotics.org/blog/2020/5/23/noetic-ninjemys-the-last-official-ros-1-release)
    1. 2020년 5월, ROS Noetic Ninjemys 배포 후 ROS1 개발 종료
- **개발자의 ROS2 관심과 [사용률이 증가](https://metrics.ros.org/packages_rosdistro.html)(2021.05 기준 ROS2 비율 20%를 넘김)하면서 변화의 흐름이 생김**
- **amazon, samsung, intel, Microsoft  같은 글로벌 기업들의 관심과 다양한 기업의 [ROS2 Technical Streering Committee](https://index.ros.org/doc/ros2/Governance/) 결성**

![https://github.com/dsy-sw/to-learning-ros-ros2/blob/main/ROS%20ROS2%20Framework/The%20reason%20for%20using%20ros2/Untitled.png)

- **ROS2만의 특징**
    1. 시장 출시 기간 단축

        로봇 개발에 필요한 도구, 라이브러리 및 기능을 제공하므로 개발에 더 많은 시간 할애가 가능

        사용할 부분과 사용 방법을 유연하게 결정 및 필요에 따라 수정 가능

    2. 생산을 위한 설계

        ROS2는 산업용 수준으로 개발되고, 높은 신뢰성과 안전을 중시

        업계 이해 관계자들로부터 얻은 요구 사항을 기반으로 개발됨

    3. 멀티 플랫폼

        Linux, Window, macOS에서 ROS2가 사용 가능함. 따라서, 자율성, 백엔드 관리 및 사용자 인터페이스의 개발 및 배포 가능

        Real Time OS 및 Embedded OS의 새로운 플랫폼으로 도입 가능

    4. 벤더 선택 가능

        ROS2부터 로봇공학 라이브러리와 응용 프로그램을 통신 기능으로부터 분리하여 추상화 작업함

        사용자가 원하는 형태로 라이브러리 및 사용자 Application 개발, 수정 가능

    5. 공개 표준 기반

        IDL, DDS, DDS-I RTPS같은 다양한 산업 표준 사용

    6. 외에도 다양한 컨셉과 특징은 [링크](https://index.ros.org/doc/ros2/_downloads/ca487a5e252ef6910bcb40402640bde6/ros2-brochure-a4-web.pdf)를 참고