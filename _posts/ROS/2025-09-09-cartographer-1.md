---
title: "[ROS 2] Cartographer SLAM 1) NeuronBot2 시뮬레이션 환경 구성"
date: 2025-09-09 12:09:00 +0900
categories: [ROS, SLAM]
---

&nbsp;

## NeuronBot2

<br>

NeuronBot2 [[1]](<https://www.adlinktech.com/Products/ROS2_Solution/ROS_Opensource_Solution/NeuronBot?lang=ko>)는 ADLINK Technology에서 개발한 로봇이다. ROS/ROS 2 라이브러리 및 패키지를 지원한다.

<br>

![NeuronBot2](/assets/img/2025-09-09/neuronbot2.png)

<br>

## NeuronBot2 설치

<br>

시뮬레이션 환경을 구성하는 데 필요한 개발 도구들을 설치한다 [[2]](<https://github.com/Adlink-ROS/neuronbot2>). (`-y` 옵션: "yes"를 자동으로 선택)

<br>

```bash
sudo apt update && sudo apt install -y \
  build-essential \
  cmake \
  git \
  libbullet-dev \
  python3-colcon-common-extensions \
  python3-flake8 \
  python3-pip \
  python3-pytest-cov \
  python3-rosdep \
  python3-setuptools \
  python3-vcstool \
  openssh-server \
  wget
```

<br>

![Screenshot 1](/assets/img/2025-09-09/cartographer-1.png)|![Screenshot 2](/assets/img/2025-09-09/cartographer-2.png)

<br>

`nav2_ws`라는 이름의 워크스페이스를 생성한다. (`-p` 옵션: 부모 디렉토리까지 함께 생성)

<br>

```bash
mkdir -p ~/nav2_ws/src
```

<br>

![Screenshot 3](/assets/img/2025-09-09/cartographer-3.png)|![Screenshot 4](/assets/img/2025-09-09/cartographer-4.png)

<br>

생성한 워크스페이스로 이동한 뒤 깃 리포지토리로부터 필요한 파일을 다운로드한다. (`vcs`: version control system)

<br>

```bash
cd ~/nav2_ws/
wget https://raw.githubusercontent.com/Adlink-ROS/neuronbot2_ros2.repos/humble/neuronbot2_ros2.repos
vcs import src < neuronbot2_ros2.repos
```

<br>

![Screenshot 5](/assets/img/2025-09-09/cartographer-5.png)|![Screenshot 6](/assets/img/2025-09-09/cartographer-6.png)

<br>

rosdep 데이터베이스를 업데이트한다. `sudo rosdep init` 명령어는 업데이트 전 최소 한 번은 실행해야 한다 [[3]](<https://ropiens.tistory.com/222>).

<br>

```bash
# sudo rosdep init
rosdep update
```

<br>

![Screenshot 7](/assets/img/2025-09-09/cartographer-7.png)|![Screenshot 8](/assets/img/2025-09-09/cartographer-8.png)|![Screenshot 9](/assets/img/2025-09-09/cartographer-9.png)|![Screenshot 10](/assets/img/2025-09-09/cartographer-10.png)

<br>

패키지를 빌드(build)하기 전에 패키지를 사용하는 데 필요한 의존성 패키지를 설치한다.

<br>

```bash
rosdep install --from-paths src --ignore-src -r -y --rosdistro humble
```

<br>

![Screenshot 11](/assets/img/2025-09-09/cartographer-11.png)|![Screenshot 12](/assets/img/2025-09-09/cartographer-12.png)

<br>

워크스페이스의 루트 디렉토리(`nav2_ws`)에서 패키지를 빌드한다.

<br>

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

<br>

![Screenshot 13](/assets/img/2025-09-09/cartographer-13.png)|![Screenshot 14](/assets/img/2025-09-09/cartographer-14.png)

<br>

터미널을 열 때마다 생성한 워크스페이스를 자동으로 활성화하기 위해 `.zshrc` 파일에 환경 변수 설정 파일을 등록한다.

<br>

```
source ~/nav2_ws/install/local_setup.zsh
```

<br>

![Screenshot 15](/assets/img/2025-09-09/cartographer-15.png)|![Screenshot 16](/assets/img/2025-09-09/cartographer-16.png)

<br>

새 터미널을 열거나 아래의 명령어를 실행한다.

<br>

```bash
source ~/nav2_ws/install/local_setup.zsh
```

<br>

![Screenshot 17](/assets/img/2025-09-09/cartographer-17.png)

<br>

`neuronbot2_gazebo` 패키지 안에 있는 `neuronbot2_world.launch.py`라는 런치(launch) 파일을 실행한다. 하지만 가제보 클라이언트(gzclient) 쪽에서 에러가 발생해 프로세스가 중단된 것을 확인할 수 있다.

<br>

```bash
ros2 launch neuronbot2_gazebo neuronbot2_world.launch.py
```

<br>

![Screenshot 18](/assets/img/2025-09-09/cartographer-18.png)|![Screenshot 19](/assets/img/2025-09-09/cartographer-19.png)

<br>

`.zshrc` 파일에 가제보가 설치된 실제 경로를 환경 변수로 등록한다. (`gazebo`와 `gazebo-11` 중 어떤 것을 선택해도 상관없다.)

<br>

```
export GAZEBO_RESOURCE_PATH=/usr/share/gazebo-11
```

<br>

![Screenshot 20](/assets/img/2025-09-09/cartographer-20.png)|![Screenshot 21](/assets/img/2025-09-09/cartographer-21.png)|![Screenshot 22](/assets/img/2025-09-09/cartographer-22.png)

<br>

새 터미널을 열어 런치 파일을 다시 한번 실행하면 사람 얼굴 형태의 환경(Mememan world)에 NeuronBot2 로봇이 정상적으로 스폰(spawn)된 것을 확인할 수 있다.

<br>

![Screenshot 23](/assets/img/2025-09-09/cartographer-23.png)

<br>

로봇이 2D LiDAR로 주변 환경을 감지하는 것을 확인할 수 있다.

<br>

![Screenshot 24](/assets/img/2025-09-09/cartographer-24.png)|![Screenshot 25](/assets/img/2025-09-09/cartographer-25.png)|![Screenshot 26](/assets/img/2025-09-09/cartographer-26.png)

<br>

시뮬레이션 환경 구성이 끝났다면 아래의 명령어를 실행해 시뮬레이션을 깔끔하게 종료한다.

<br>

```bash
killgazebo
```

<br>

## 시뮬레이션 환경 변경

<br>

런치 파일을 실행했을 때 Mememan 환경이 아닌 다른 환경을 띄울 수 있다.

<br>

런치 파일에서 `mememan_world.model`을 `phenix_world.model`로 수정한다.

(런치 파일 경로:`nav2_ws/src/neuronbot2/neuronbot2_gazebo/launch/neuronbot2_world.launch.py`)

<br>

![Screenshot 2](/assets/img/2025-09-10/screenshot-2.png)|![Screenshot 3](/assets/img/2025-09-10/screenshot-3.png)

<br>

다시 런치 파일을 실행해 보면 Phenix 환경이 열리는 것을 확인할 수 있다.

<br>

![Screenshot 4](/assets/img/2025-09-10/screenshot-4.png)

<br>

---

## References

[1] <https://www.adlinktech.com/Products/ROS2_Solution/ROS_Opensource_Solution/NeuronBot?lang=ko>  
[2] <https://github.com/Adlink-ROS/neuronbot2>  
[3] <https://ropiens.tistory.com/222>