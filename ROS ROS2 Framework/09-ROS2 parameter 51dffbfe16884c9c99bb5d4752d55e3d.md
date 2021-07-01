# 09-ROS2 parameter

# 1. 파라미터(parameter)

![09-ROS2%20parameter%2051dffbfe16884c9c99bb5d4752d55e3d/Untitled.png](09-ROS2%20parameter%2051dffbfe16884c9c99bb5d4752d55e3d/Untitled.png)

- ROS2에서 파라미터(parameter)는 각 노드에서 파라미터 관련 Parameter server를 실행시켜 그림과 같이 외부의 Parameter client와 통신으로 파라미터를 변경하는 것
- 노드 내 매개변수를 서비스 데이터 통신 방법을 사용하여 노드 내/외부에서 지정(Set)하거나 변경, 쉽게 가져(Get)와 사용하는 점이 파라미터의 사용 목적이지만 서비스는 서비스 요청과 응답이라는 PRC(remote procedure call)가 목적이다.
- 모든 노드가 자신만의 parameter server를 가지며 parameter client도 포함할 수 있어 자신의 파라미터와 다른 노드의 파라미터를 읽고 쓸 수 있다.
- yaml 파일 형태의 파라미터 설정 파일을 만들어 초기 파라미터 값 설정 및 노드 실행 시 파라미터 설정 파일을 불러와 사용 가능하다.

# 2. 파라미터 목록 확인(ros2 param list)

```bash
$ ros2 run turtlesim turtlesim_node
```

```bash
$ ros2 run turtlesim turtle_teleop_key
```

```bash
$ ros2 param list
/teleop_turtle:
  scale_angular
  scale_linear
  use_sim_time
/turtlesim:
  background_b
  background_g
  background_r
  use_sim_time
```

- 'ros2 param list' 명령어로 현재 개발 환경에서 실행 중인 노드(turtlesim, teleop_turtle)의  parameter server에 접근하여 현재 사용 가능한 파라미터를 노드별로 목록화하여 표시된다.

# 3. 파라미터 내용 확인(ros2 param descirbe)

```bash
$ ros2 param describe /turtlesim background_b
Parameter name: background_b
  Type: integer
  Description: Blue channel of the background color
  Constraints:
    Min value: 0
    Max value: 255
    Step: 1
```

- 'ros2 param descibe' 명령어에 노드 이름과 파라미터 이름을 지정하면 파라미터의 형태, 목적, 데이터 형태, 최소/최대값을 확인할 수 있다.

# 4. 파라미터 읽기(ros2 param get)

> ros2 param get <node_name> <parameter_name>

- 'ros2 param get' 명령어에 노드 이름과 파라미터 이름을 적으면 해당 파라미터를 읽을 수 있다.

```bash
$ ros2 param get /turtlesim background_r
Integer value is: 69

$ ros2 param get /turtlesim background_g
Integer value is: 86

$ ros2 param get /turtlesim background_b
Integer value is: 255
```

- turtlesim 노드의 background r,g,b 파라미터 값을 읽어오면 r=69, g=86, b=255인 것을 확인할 수 있다.

# 5. 파라미터 쓰기(ros2 param set)

> ros2 param set <node_name> <parameter_name> <value>

- 'ros2 param set' 명령어에 노드 이름, 파라미터 이름, 변경할 파라미터 값을 지정하면 파라미터 쓰기가 실행된다.

```bash
$ ros2 param set /turtlesim background_r 148
Set parameter successful

$ ros2 param set /turtlesim background_g 0
Set parameter successful

$ ros2 param set /turtlesim background_b 211
Set parameter successful
```

![09-ROS2%20parameter%2051dffbfe16884c9c99bb5d4752d55e3d/Untitled%201.png](09-ROS2%20parameter%2051dffbfe16884c9c99bb5d4752d55e3d/Untitled%201.png)

![09-ROS2%20parameter%2051dffbfe16884c9c99bb5d4752d55e3d/Untitled%202.png](09-ROS2%20parameter%2051dffbfe16884c9c99bb5d4752d55e3d/Untitled%202.png)

- turtlesim의 background r,g,b 파라미터 값을 r=148, g=0, b=211로 변경하면 별도의 컴파일 과정 없이 우측의 Dark Violet 색상으로 변경하는 것을 볼 수 있다.

# 6. 파라미터 저장(ros2 param dump)

```bash
$ ros2 param dump /turtlesim
Saving to: ./turtlesim.yaml
```

- 'ros2 param dump' 명령어에 노드 이름을 기재하면 핸자 폴더에 해당 노드의 이름과 yaml 형태로 파일이 저장된다.

```bash
$ cat ./turtlesim.yaml
turtlesim:
  ros__parameters:
    background_b: 211
    background_g: 0
    background_r: 148
    use_sim_time: false
```

- 해당 파일에는 turtlesim 노드가 파라미터 서보로하여 가지는 background r,g,b와 use_sim_time 등 4개의 파라미터 값이 저장된다.

```bash
$ ros2 run turtlesim turtlesim_node --ros-args --params-file ./turtlesim.yaml
```

- '—ros-args —params-file' 옵션을 추가하여 yaml 파일 위치를 입력하면 해당 파일이 노드의 파라미터 기본 값으로 설정된다.

# 7. 파라미터 삭제(ros2 param delete)

```bash
$ ros2 param delete /turtlesim background_b
Deleted parameter successfully
```

- 'ros2 param delete' 명령어에 노드 이름과 파라미터 이름을 적어주면 특정 파라미터가 삭제된다.