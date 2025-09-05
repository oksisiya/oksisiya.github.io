---
title: "[ROS 2] ROS 2 통신 2) Interface - 개념과 명령어"
date: 2025-09-05 10:05:00 +0900
categories: [ROS, Basic]
---

&nbsp;

## Interface란?

<br>

Interface는 ROS에서 Node 간 주고받는 **데이터의 구조**를 정의한다.

<br>

ROS에서는 일반적으로 Message, Service, Action 중 하나의 Interface를 통해 통신한다 [[1](<https://hyundoil.tistory.com/430>)].
* **Message** (.msg): Topic을 통한 **단방향 통신**에 사용
* **Service** (.srv): 요청-응답(Request-Response) 모델의 **양방향 통신**에 사용
* **Action** (.action): 장기 실행 작업을 위한 **비동기 양방향 통신**에 사용

<br>

각각의 Interface는 괄호 안의 확장자를 갖는 텍스트 파일로 정의한다.

<br>

![Interface](/assets/img/2025-09-05/interface.jpeg)

<br>

## Interface 명령어

<br>

Interface 명령어를 확인하기에 앞서 먼저 TIAGo 로봇 시뮬레이션 환경을 열어놓는다.

<br>

```bash
ros2 launch tiago_gazebo tiago_gazebo.launch.py is_public_sim:=True
```

<br>

![TIAGo simulation 1](/assets/img/2025-09-05/tiago-simulation-1.png)|![TIAGo simulation 2](/assets/img/2025-09-05/tiago-simulation-2.png)

<br>

### ros2 interface list

<br>

현재 개발 환경에서 사용 가능한 Interface의 목록을 확인할 수 있다.

<br>

```bash
ros2 interface list
```

<br>

![Interface command 1](/assets/img/2025-09-05/interface-command-1.png)|![Interface command 2](/assets/img/2025-09-05/interface-command-2.png)|![Interface command 3](/assets/img/2025-09-05/interface-command-3.png)

<br>

### ros2 interface show

<br>

특정 Interface의 데이터 구성을 확인할 수 있다.

<br>

```bash
ros2 interface show <interface_name>
```

<br>

지난 포스팅에서 `/cmd_vel` Topic의 Interface 타입이 `geometry_msgs/msg/Twist`임을 확인했다. `Twist`는 로봇의 속도를 정의하는 Message 타입이다. Vector3 형식으로 구성되어 세 가지(x, y, z) 축에 대한 선속도(linear)와 각속도(angular)를 나타낸다 [[2]](https://kasimov.korea.ac.kr/wiki/doku.php/activity/public/2021/ros/6).

<br>

```bash
ros2 interface show geometry_msgs/msg/Twist
```

<br>

![Interface command 4](/assets/img/2025-09-05/interface-command-4.png)

<br>

### ros2 interface proto

<br>

특정 Interface의 프로토타입(prototype)을 확인할 수 있다.

<br>

```bash
ros2 interface proto <interface_name>
```

<br>

`ros2 interface show` 명령어와 달리 자료형(`float64`)이 아닌 각 축에 대한 구체적인 값을 확인할 수 있다.

<br>

```bash
ros2 interface proto geometry_msgs/msg/Twist
```

<br>

![Interface command 5](/assets/img/2025-09-05/interface-command-5.png)

<br>

---

## References

[1] <https://hyundoil.tistory.com/430>  
[2] <https://kasimov.korea.ac.kr/wiki/doku.php/activity/public/2021/ros/6>