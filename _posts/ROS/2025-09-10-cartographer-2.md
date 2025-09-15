---
title: "[ROS 2] Cartographer SLAM 2) 지도 작성과 지도 저장"
date: 2025-09-10 10:30:00 +0900
categories: [ROS, SLAM]
---

&nbsp;

## Cartographer

<br>

Cartographer [[1]](<https://google-cartographer.readthedocs.io/en/latest/>)는 다양한 플랫폼과 센서 구성에서 실시간 2D 및 3D SLAM 기능을 제공하는 시스템이다.

<br>

## Cartograper 관련 파일

<br>

Cartographer로 SLAM을 수행하기 위해 필요한 세 가지 파일이 있다.

<br>

### Launch 파일

<br>

* 파일 경로: `~/nav2_ws/src/neuronbot2/neuronbot2_slam/launch/cartographer_launch.py`
* 파일 설명: Cartographer SLAM과 관련된 노드들을 실행하기 위한 스크립트

<br>

Cartographer SLAM을 수행하려면 런치 파일을 수정해야 한다. 런치 파일을 통해 실행하고자 하는 파일 이름이 잘못 지정되어 있기 때문이다. 실행 파일이 존재하는 디렉토리✳︎를 직접 확인해 보면 실행 파일 이름(`cartographer_occupancy_grid_node`)과 런치 파일에 지정된 파일 이름(`occupancy_grid_node`)이 다른 것을 확인할 수 있다.

✳︎ 파일 경로: `Computer/opt/ros/humble/lib/cartographer_ros/cartographer_occupancy_grid_node`

<br>

다음과 같이 실행 파일 이름과 노드 이름을 변경한다.

* `executable='occupancy_grid_node'` => `'cartographer_occupancy_grid_node'`
* `name='occupancy_grid_node'` => `'cartographer_occupancy_grid_node'`

<br>

![Screenshot 5](/assets/img/2025-09-10/screenshot-5.png)|![Screenshot 6](/assets/img/2025-09-10/screenshot-6.png)|![Screenshot 7](/assets/img/2025-09-10/screenshot-7.png)

<br>

### Lua 파일

<br>

* 파일 경로: `~/nav2_ws/src/neuronbot2/neuronbot2_slam/config/cartographer.lua`
* 파일 설명: Cartographer SLAM 알고리즘에 영향을 주는 값들을 포함하는 설정 파일

<br>

런치 파일에는 Cartographer 노드를 실행하기 위해 필수로 포함해야 하는 인자(arguments)로 `cartographer_config_dir`와 `configuration_basename`이 있다.

<br>

```python
cartographer_config_dir = LaunchConfiguration('cartographer_config_dir', default=os.path.join(get_package_share_directory('neuronbot2_slam'), 'config'))
configuration_basename = LaunchConfiguration('configuration_basename', default='cartographer.lua')
```

<br>

그중 `configuration_basename`의 기본값이 Lua 포맷으로 기술된 설정(config) 파일(`cartographer.lua`)임을 확인할 수 있다. ROS 2에서 config 파일은 대부분 YAML 포맷으로 기술하지만 Cartographer의 경우 Lua 포맷으로 config 파일을 기술한다.

<br>

![Screenshot 8](/assets/img/2025-09-10/screenshot-8.png)|![Screenshot 9](/assets/img/2025-09-10/screenshot-9.png)

<br>

### Rviz 파일

<br>

* 파일 경로: `~/nav2_ws/src/neuronbot2/neuronbot2_slam/rviz/slam.rviz`
* 파일 설명: Rviz 시각화와 관련된 설정 파일

<br>

통합 터미널을 열고 아래의 명령어를 실행해 Rivz 파일을 확인해 보면 Displays에서 시각화와 관련된 설정들을 확인할 수 있다.

<br>

```bash
rviz2 -d slam.rviz
```

<br>

![Screenshot 10](/assets/img/2025-09-10/screenshot-10.png)|![Screenshot 11](/assets/img/2025-09-10/screenshot-11.png)|![Screenshot 12](/assets/img/2025-09-10/screenshot-12.png)

<br>

## 지도 작성

<br>

지도 작성은 크게 세 단계로 이루어진다.

<br>

### 시뮬레이션 환경 열기

<br>

첫 번째 터미널을 열어 런치 파일을 실행한다.

<br>

```bash
ros2 launch neuronbot2_gazebo neuronbot2_world.launch.py
```

<br>

![Screenshot 13](/assets/img/2025-09-10/screenshot-13.png)|![Screenshot 14](/assets/img/2025-09-10/screenshot-14.png)|![Screenshot 15](/assets/img/2025-09-10/screenshot-15.png)

<br>

### Cartographer SLAM 수행

<br>

두 번째 터미널을 열어 Cartographer SLAM을 수행한다.

<br>

```bash
ros2 launch neuronbot2_slam cartographer_launch.py open_rviz:=true use_sim_time:=true
```

<br>

![Screenshot 16](/assets/img/2025-09-10/screenshot-16.png)|![Screenshot 17](/assets/img/2025-09-10/screenshot-17.png)|![Screenshot 18](/assets/img/2025-09-10/screenshot-18.png)

<br>

### 로봇 조종

<br>

세 번째 터미널을 열어 로봇을 지도 상에서 움직인다. (실제 환경에서 맵핑할 때의 팁은 로봇을 천천히 움직이는 것이다.)

<br>

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

<br>

![Screenshot 19](/assets/img/2025-09-10/screenshot-19.png)|![Screenshot 20](/assets/img/2025-09-10/screenshot-20.png)|![Screenshot 21](/assets/img/2025-09-10/screenshot-21.png)

<br>

지도는 점유 격자 지도(occupancy grid map) 형태로 작성된다.

<br>

![Cartographer SLAM 1](/assets/img/2025-09-10/cartographer-slam-1.png)|![Cartographer SLAM 2](/assets/img/2025-09-10/cartographer-slam-2.png)|![Cartographer SLAM 3](/assets/img/2025-09-10/cartographer-slam-3.png)

<br>

## 지도 저장

<br>

* 지도 저장 경로: `~/nav2_ws/src/neuronbot2/neuronbot2_nav/map`

<br>

지도를 저장할 디렉토리를 확인해 보면 패키지 내에서 사전 정의된 지도들이 있다.

<br>

![Screenshot 22](/assets/img/2025-09-10/screenshot-22.png)

<br>

2D LiDAR 기반의 SLAM 알고리즘을 통해 작성한 지도를 저장하려면 `nav2_map_server` 패키지의 `map_saver` 노드를 실행한다.

<br>

```bash
ros2 run nav2_map_server map_saver_cli -f phenix_my.pgm
```

<br>

![Screenshot 23](/assets/img/2025-09-10/screenshot-23.png)|![Screenshot 24](/assets/img/2025-09-10/screenshot-24.png)

<br>

`map` 디렉토리에 두 개의 파일(`phenix_my.pgm`과 `phenix_my.yml`) 파일이 생성된 것을 확인할 수 있다.

<br>

![Screenshot 25](/assets/img/2025-09-10/screenshot-25.png)

<br>

두 파일에 대한 설명은 다음과 같다.

* `<file_name>.pgm`
    - 회색 음영과 픽셀 단위로 2D 환경의 지도를 표현하는 이미지 파일 
    - 영상 파일 확장자 중 하나 (`.pgm`: portable gray map)
* `<file_name>.yml`
    - 지도 설정과 관련된 파일
    - 네비게이션 패키지(`nav2` 등)에 필요한 메타데이터를 포함

<br>

![Screenshot 26](/assets/img/2025-09-10/screenshot-26.png)|![Screenshot 27](/assets/img/2025-09-10/screenshot-27.png)

<br>

`<file_name>.yml`의 파라미터에 대한 설명은 다음과 같다. (지도 상에서 원점은 왼쪽 하단에 위치한다.)

<br>

|파라미터|설명|`phenix_my.yml`의 해석|
|-|-|-|
|image|설정 파일과 연결된 지도 파일||
|mode|지도 해석 방식|지도를 세 가지 영역으로 구분<br>(점유 영역, 자유 영역, 미지의 영역)|
|resolution|지도 해상도 ([m])|한 픽셀 당 5cm|
|origin|map 좌표계 기준점 (x, y, θ)|원점은 map frame을 기준으로 (-3.82, -15.2)에 위치|
|negate|색 반전 여부 (default: 0)|점유 영역: 검정색 / 자유 영역: 흰색 / 미지의 영역: 회색<br>(1로 설정할 경우 점유와 자유가 반대)|
|occupied_thresh|점유 공간 판정 기준|픽셀 값이 0.65보다 크다면 점유 영역|
|free_thresh|자유 공간 판정 기준|픽셀 값이 0.25보다 작다면 자유 영역|

<br>

생성된 지도에 대해 워크스페이스의 루트 디렉토리(`nav2_ws`)에서 패키지를 빌드한다.

<br>

```bash
cd ~/nav2_ws/
colcon build --symlink-install --packages-select neuronbot2_nav
```

<br>

![Screenshot 29](/assets/img/2025-09-10/screenshot-29.png)

<br>

---

## References

[1] <https://google-cartographer.readthedocs.io/en/latest/>  
[2] <https://keep-steady.tistory.com/47>