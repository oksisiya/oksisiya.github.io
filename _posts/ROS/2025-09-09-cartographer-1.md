---
title: "[ROS 2] Cartographer SLAM 1) NeuronBot2 시뮬레이션 환경 구성"
date: 2025-09-09 12:09:00 +0900
categories: [ROS, SLAM]
---

&nbsp;

이번 포스팅에서는 지도를 작성하기에 앞서 시뮬레이션 환경을 구성한다.

<br>

## NeuronBot2

<br>

NeuronBot2 [[1]](<https://www.adlinktech.com/Products/ROS2_Solution/ROS_Opensource_Solution/NeuronBot?lang=ko>)는 ADLINK Technology에서 개발한 로봇이다. ROS/ROS 2 라이브러리 및 패키지를 지원한다.

<br>

![NeuronBot2](/assets/img/2025-09-09/neuronbot2.png)

<br>

## NeuronBot2 설치

<br>

시뮬레이션 환경을 구성하는 데 필요한 개발 도구들을 설치한다 [[2]](<https://github.com/Adlink-ROS/neuronbot2>).

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

`nav2_ws`라는 이름의 워크스페이스를 생성한다.

<br>

```bash
mkdir -p ~/nav2_ws/src
```

<br>

![Screenshot 3](/assets/img/2025-09-09/cartographer-3.png)|![Screenshot 4](/assets/img/2025-09-09/cartographer-4.png)

<br>

생성한 워크스페이스로 이동한 뒤 깃 리포지토리로부터 필요한 파일을 다운로드한다.

<br>

```bash
cd ~/nav2_ws/
wget https://raw.githubusercontent.com/Adlink-ROS/neuronbot2_ros2.repos/humble/neuronbot2_ros2.repos
vcs import src < neuronbot2_ros2.repos
```

<br>

![Screenshot 5](/assets/img/2025-09-09/cartographer-5.png)|![Screenshot 6](/assets/img/2025-09-09/cartographer-6.png)

<br>

rosdep 데이터베이스를 업데이트한다. `sudo rosdep init` 명령어는 업데이트 전 최소 한 번은 실행되어야 한다 [[3]](<https://ropiens.tistory.com/222>).

<br>

```bash
# sudo rosdep init
rosdep update
```

<br>

![Screenshot 7](/assets/img/2025-09-09/cartographer-7.png)|![Screenshot 8](/assets/img/2025-09-09/cartographer-8.png)|![Screenshot 9](/assets/img/2025-09-09/cartographer-9.png)|![Screenshot 10](/assets/img/2025-09-09/cartographer-10.png)

<br>

다른 의존성 패키지들도 설치한다.

<br>

```bash
rosdep install --from-paths src --ignore-src -r -y --rosdistro humble
```

<br>

![Screenshot 11](/assets/img/2025-09-09/cartographer-11.png)|![Screenshot 12](/assets/img/2025-09-09/cartographer-12.png)

<br>

패키지를 빌드한다.

<br>

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

<br>

![Screenshot 13](/assets/img/2025-09-09/cartographer-13.png)|![Screenshot 14](/assets/img/2025-09-09/cartographer-14.png)

<br>

`.zshrc` 파일에 아래의 명령어를 추가한다.

<br>

```
source ~/nav2_ws/install/local_setup.zsh
```

<br>

![Screenshot 15](/assets/img/2025-09-09/cartographer-15.png)|![Screenshot 16](/assets/img/2025-09-09/cartographer-16.png)

<br>

터미널에 아래의 명령어를 입력하거나 새로운 터미널 창을 연다.

<br>

```bash
source ~/nav2_ws/install/local_setup.zsh
```

<br>

![Screenshot 17](/assets/img/2025-09-09/cartographer-17.png)

<br>

NeuronBot2 시뮬레이션을 실행시키려고 하면 에러가 나고 열리지 않는다.

<br>

```bash
ros2 launch neuronbot2_gazebo neuronbot2_world.launch.py
```

<br>

![Screenshot 18](/assets/img/2025-09-09/cartographer-18.png)|![Screenshot 19](/assets/img/2025-09-09/cartographer-19.png)

<br>

그럴 땐 ~...

<br>

![Screenshot 20](/assets/img/2025-09-09/cartographer-20.png)|![Screenshot 21](/assets/img/2025-09-09/cartographer-21.png)|![Screenshot 22](/assets/img/2025-09-09/cartographer-22.png)

<br>

마침내 시뮬레이션이 정상적으로 실행된 것을 확인할 수 있다.

<br>

![Screenshot 23](/assets/img/2025-09-09/cartographer-23.png)

<br>

라이다~... 뉴런봇2를 확인할 수 있다.

<br>

![Screenshot 24](/assets/img/2025-09-09/cartographer-24.png)|![Screenshot 25](/assets/img/2025-09-09/cartographer-25.png)|![Screenshot 26](/assets/img/2025-09-09/cartographer-26.png)

<br>

---

## References

[1] <https://www.adlinktech.com/Products/ROS2_Solution/ROS_Opensource_Solution/NeuronBot?lang=ko>  
[2] <https://github.com/Adlink-ROS/neuronbot2>  
[3] <https://ropiens.tistory.com/222>