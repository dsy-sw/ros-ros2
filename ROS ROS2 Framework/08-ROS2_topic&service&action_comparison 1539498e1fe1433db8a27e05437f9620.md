# 08-ROS2_topic&service&action_comparison

# 1. ROS2 토픽/서비스/액션

- 토픽/서비스/액션을 모두 모아 정리하고 비교하여 이해하고자 한다.

# 2. 토픽(topic)

![08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled.png](08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled.png)

- 토픽은 Node A - B 처럼 비동기식 단방향 메시지 송수신 방식으로 msg 메시지 형태의 메시지를 발행하는 Publisher와 메시지를 구독하는 'Subscriber' 간의 통신이다.
- 1:1 통신을 기본으로  1:N, N:1, N:N 통신도 가능하다.
- Node A처럼 하나 이상의 토픽을 발행할 수 있을 뿐만 아니라 Publisher기능과 Subscriber 역할도 동시에 수행할 수 있다.
- 비동기성과 연속성을 가지기에 센서 값 전송 및 항시 정보를 주고 받아야 하는 부분에서 사용된다.

# 3. 서비스(service)

![08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled%201.png](08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled%201.png)

- Node A - B처럼 동기식 양방향 메시지 송수신 방식으로 서비스의 요청(request)을 하는 쪽을 service client, 요청받은 서비스를 실행한 후 서비스의 응답(response)하는 쪽을 service server라고 한다.
- - 서비스는 특정 여청을 하는 클라이언트 단과 요청받은 일을 수행 후 결과값을 전달하는 서버 단과의 통신이며, msg 인터페이스의 변형으로 srv 인터페이스라고 한다.
- 동일 서비스에 대해 복수의 클라이언트를 가질 수 있지만, 서비스 응답은 서비스 요청이 있었던 서비스 클라이언트에 대해서만 응답할 수 있다.

# 4. 액션(action)

![08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled%202.png](08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled%202.png)

- Node A - B처럼 비동기식 + 동기식 양방향 메시지 송수신 방식으로 액션 목표(goal)를 지정하는 action client와 목표를 받아 특정 태스크를 수행하면서 중간 결과값에 해당되는 액션 피드백(feedback)과 최종 결과값에 해당되는 액션 결과(result)를 전송하는 action server간의 통신이다.
- 액션이 구현방식을 보면 토픽과 서비스의 혼합으로 볼 수 있다. ation client는 service client 3개와 topic subscriber 2개로 구성되며, action server는 service server 3개와 topic publisher 2개로 구성된다.

![08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled%203.png](08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled%203.png)

- ROS2부터 목표 상태(goal_state) 및 피드백(feedback)은 비동기식의 토픽, 목표 전달(send_goal)와 목표 취소(cancel_goal)와 결과받기(get_result)는 동기식인 서비스를 이용하며 토픽과 서비스 방식을 혼합하여 사용하였다. 그리고 이를 원활히 구현하기 위해ㅔ 액션 프로세스 내에 상태 머신(state machine)을 선보였다.
- 액션 목표 전달 이후 액션의 상태값을 액션 클라이언트에게 전달할 수 있어 동기, 비동기 방식이 혼재된 액션의 처리를 원활하게 가능하다.

# 5. 토픽, 서비스, 액션 비교

![08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled%204.png](08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled%204.png)

![08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled%205.png](08-ROS2_topic&service&action_comparison%201539498e1fe1433db8a27e05437f9620/Untitled%205.png)

- 연속성, 방향성, 동기성, 다자간 연결, 노드 역할, 동작 트리거, 인터페이스를 각 토픽, 서비스, 액션의 서로 다른 특징으로 볼 수 있고 노드 간의 데이터 전송에 있어서 특성에 맞게 선택하여 프로그래밍하면 된다.