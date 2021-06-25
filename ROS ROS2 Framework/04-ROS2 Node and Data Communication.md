# 04-ROS2 Node and Data Communication

# 1. node and message communication

- Node는 최소 단위의 실행 가능한 프로세스로서 하나의 실행 가능한 프로그램을 이야기하는 것으로 ROS에서는 최소한의 실행 단위로 프로그램을 나누어 작업한다.

- 수많은 노드들이 연동되는 ROS 시스템을 위해서는 노드와 노드 사이에 입력과 출력 데이터를 서루 주고받게 설계해야 하며 이때 주고받는 데이터를 메시지, 주고받는 방식을 메시지 통신이라 한다. 메시지는 integer, floating point, boolean, string같은 변수 형태이며, 메시지를 주고 받는 통신 방법에 따라 1. 토픽, 2. 서비스, 3. 액션, 4. 파라미터로 나뉘게 된다.

  ![04-ROS2%20Node%20and%20Data%20Communication/Untitled.png](04-ROS2%20Node%20and%20Data%20Communication/Untitled.png)

  - 토픽(Topic)  : 위 그림 Node A - B, Node A - C 처럼 비동기식 단방향 메시지 송수신 방식으로 메시지 Publisher와 Subscriber 간의 통신. 1:N, N:1, N:N 통신이 가능하며 ROS 메시지 통신에서 가장 널리 사용.
  - 서비스(Service) : Node B-C처럼 동기식 양방향 메시지 송수신 방식으로 서비스의 요청(Request)을 하는 쪽을 Service Client이며, 서비스의 응답(Response)을 하는 쪽을 Service Server이다. 서비스 요청 및 응답(Request/Response) 또한 위에서 언급한 msg 메시지의 변형으로 srv 메시지라고 한다.
  - 액션(Action) : Node A - B 처럼 비동기식 + 동기식 양방향 메시지 송수신 방식으로 액션 목표(Goal)를 지정하는 Action client와 액션 목표를 받아 특정 task를 수행하면서 중간 결과값에 해당되는 액션 피드백(Action Feedback)과 최종 결과값에 해당되는 액션 결과(Action Result)를 전송하는 Action server 간의 통신. 액션 목표 및 결과를 전달하는 방식은 서비스, 피드백은 토픽과 같은 메시지 전송 방식이다. 각 메시지는 msg 메시지의 변형으로 action 메시지라고 한다.
  - 파라미터(Parameter) : 각 노드에 Parameter server를 실행시켜 외부의 clinet 간의 통신으로 파라미터를 변경하는 것은 서비스와 동일하지만, 노드 내 매개변수 또는 글로벌 매개변수를 서비스 메시지 통신 방법을 사용하여 노드 내부 또는 외부에서 쉽게 지정(set)하거나 변경할 수 있고, 쉽게 가져와서 사용(get)할 수 있게 하는 점에서 목적이 다르다.

# 2. 노드 실행(ros2 run)

- ros2 run 명령어로 특정 패키지의 노드를 실행할 수 있다.

```bash
$ ros2 run turtlesim turtlesim_node

$ ros2 run turtlesim turtle_teleop_key
```

```bash
$ rqt_graph
```

![04-ROS2%20Node%20and%20Data%20Communication/Untitled%201.png](04-ROS2%20Node%20and%20Data%20Communication/Untitled%201.png)

- ros2 run 또는 ros2 launch 이에도 rqt, rqt_graph, rviz2같은 실행 명령어 방법이 있다.

# 3. 노드 목록(ros2 node list)

```bash
$ ros2 node list
/rqt_gui_py_node_86048
/teleop_turtle
/turtlesim
```

- 'ros2 node list' 명령어로 동작 중인 노드의 목록을 확인할 수 있으며, 현재 rqt 툴을 포함하여 3개의 노드 목록을 확인할 수 있다.

```bash
$ ros2 run turltesim turtlesim_node __node:=new_turtle
```

- 해당 문구를 실행하면 node list에 new_turtle이 추가된 것을 확인할 수 있고, rqt_graph에도 실행된 것을 확인할 수 있다. 두 노드의 거북이가 teleop_turtle 노드의 동일한 토픽을 이용하고 있어서 동일하게 움직이는 것을 볼 수 있다.

```bash
$ ros2 node list
/new_turtle
/rqt_gui_py_node_86048
/teleop_turtle
/turtlesim
```

![04-ROS2%20Node%20and%20Data%20Communication%20594187aef73d4c098fb39091dde56688/Untitled%202.png](04-ROS2%20Node%20and%20Data%20Communication/Untitled%202.png)

# 4. 노드 정보(ros2 node info)

- 'ros2 node info' 로 Publisher, Service, Subscriber, Action, Parameter 등 노드의 정보를 확인할 수 있다.

```bash
$ ros2 node info /turtlesim
/turtlesim
  Subscribers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
  Publishers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /rosout: rcl_interfaces/msg/Log
    /turtle1/color_sensor: turtlesim/msg/Color
    /turtle1/pose: turtlesim/msg/Pose
  Service Servers:
    /clear: std_srvs/srv/Empty
    /kill: turtlesim/srv/Kill
    /reset: std_srvs/srv/Empty
    /spawn: turtlesim/srv/Spawn
    /turtle1/set_pen: turtlesim/srv/SetPen
    /turtle1/teleport_absolute: turtlesim/srv/TeleportAbsolute
    /turtle1/teleport_relative: turtlesim/srv/TeleportRelative
    /turtlesim/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /turtlesim/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /turtlesim/get_parameters: rcl_interfaces/srv/GetParameters
    /turtlesim/list_parameters: rcl_interfaces/srv/ListParameters
    /turtlesim/set_parameters: rcl_interfaces/srv/SetParameters
    /turtlesim/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
  Service Clients:

  Action Servers:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
  Action Clients:
```

```bash
$ ros2 node info /teleop_turtle 
/teleop_turtle
  Subscribers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
  Publishers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /rosout: rcl_interfaces/msg/Log
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
  Service Servers:
    /teleop_turtle/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /teleop_turtle/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /teleop_turtle/get_parameters: rcl_interfaces/srv/GetParameters
    /teleop_turtle/list_parameters: rcl_interfaces/srv/ListParameters
    /teleop_turtle/set_parameters: rcl_interfaces/srv/SetParameters
    /teleop_turtle/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
  Service Clients:

  Action Servers:

  Action Clients:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
```

