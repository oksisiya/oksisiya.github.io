---
title: ROS 2 Humble 설치
date: 2025-03-25
categories: ROS
---

&nbsp;

Ubuntu 20.04를 사용하는 경우 Humble 버전이 아닌 Foxy 버전을 선호할 수 있다. 하지만 Foxy 버전의 경우 배포판 유지 관리가 중단된 지 오래이므로 최신 기능(Nav2 등)을 사용하고자 한다면 Humble 버전을 추천한다.

<br>

![Ros distro maintenance](/assets/img/2025-03-25/ros-distro-maintenance.png)
(이미지 출처: https://metrics.ros.org/rosdistro_rosdistro.html)

<br>

코드 박스의 명령어를 한 줄씩 실행한다. 명령어 실행이 완료된 터미널 창을 해당 코드 박스 밑에 첨부했다. 전체적인 과정은 [ROS 2 공식 문서](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)를 참고했다.

<br>

---

## 1. Setup Sources

<br>

아래 명령어를 통해 Ubuntu 시스템에서 Universe 리포지토리를 활성화한다. `software-properties-common`은 대부분 이미 설치되어 있을 것이다.

<br>

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

<br>

![Software-properties-common](/assets/img/2025-03-25/software-properties-common.png)|![Add universe](/assets/img/2025-03-25/add-universe.png)

<br>

curl 패키지를 설치하고 curl을 사용해 GPG 키를 등록한다.

<br>

```bash
sudo apt update && sudo apt install curl
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

<br>

![Install curl](/assets/img/2025-03-25/install-curl.png)|![Add GPG key](/assets/img/2025-03-25/add-gpg-key.png)

<br>

ROS 2 패키지 저장소를 Ubuntu 시스템의 apt 소스 목록에 추가한다. [링크]만 긁어 접속해 보면 어떤 파일들을 설치하는지 확인할 수 있다. 명령어 실행 후 아무것도 뜨지 않는다면 파일들을 정상적으로 PC에 가져온 것이다. ~~(긴 명령어는 긁어다 복사해서 붙여 넣는 것이 정확하다. ㅎㅎ)~~

<br>

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```
<br>

![Open Source Lab](/assets/img/2025-03-25/open-source-lab.png)|![Add to sources list](/assets/img/2025-03-25/echo-deb.png)

<br>

---

## 2. Install ROS 2 Packages

<br>

아래 명령어 실행 후 ROS 2와 관련된 링크(ex. GET:2, GET:6)가 나타난다면 ROS 2 설치 준비가 완료된 것이다.

<br>

```bash
sudo apt update
```

<br>

![sudo apt update](/assets/img/2025-03-25/sudo-apt-update.png)

<br>

`ros-humble-desktop` 패키지를 설치한다. 2,945 MB로, 용량이 꽤 크다. 이 단계는 PC 사양에 따라서 오래 걸릴 수도 금방 끝날 수도 있다. ~~(나는 상당히 오래 걸려서 책을 몇 페이지 읽었다.)~~  
`ros-humble-desktop` 패키지를 설치하면 Gazebo부터 RViz, 필수적인 ROS 2 패키지들을 다운로드할 수 있다. 모든 필수 패키지들을 설치하고 싶지 않다면, `desktop` 패키지가 아닌 공식 홈페이지를 통해 최소한으로 설치하는 방법도 있다.

<br>

```bash
sudo apt install ros-humble-desktop
```

<br>

![ros-humble-desktop-1](/assets/img/2025-03-25/ros-humble-desktop-1.png)|![ros-humble-desktop-2](/assets/img/2025-03-25/ros-humble-desktop-2.png)

<br>

컴파일러나 ROS 2 패키지를 빌드하는 데 필요한 개발 도구들을 설치한다. `ros-humble-desktop` 패키지보다는 가볍기 때문에 금방 설치된다.

<br>

```bash
sudo apt install ros-dev-tools
```

<br>

![ros-dev-tools](/assets/img/2025-03-25/ros-dev-tools.png)

<br>

---

## 3. Install Other Packages

<br>

ROS 2 패키지 외에 개발에 필요한 패키지들을 설치한다.

<br>

```bash
sudo apt update && sudo apt install -y build-essential cmake git libbullet-dev python3-colcon-common-extensions python3-flake8 python3-pip python3-pytest-cov python3-rosdep python3-setuptools python3-vcstool wget python3-argcomplete
python3 -m pip install -U flake8-blind-except flake8-builtins flake8-class-newline flake8-comprehensions flake8-deprecated flake8-docstrings flake8-import-order flake8-quotes pytest-repeat pytest-rerunfailures pytest
sudo apt install --no-install-recommends -y libasio-dev libtinyxml2-dev libcunit1-dev
```

<br>

![Install other packages](/assets/img/2025-03-25/install-other-packages-1.png)|![Install other packages](/assets/img/2025-03-25/install-other-packages-2.png)|![Install other packages](/assets/img/2025-03-25/install-other-packages-3.png)

<br>

---

## 4. Check Installation

<br>

ROS 2 Humble 버전이 정상적으로 설치되었는지 확인한다. 새로운 터미널을 열어 아래 명령어를 실행한다.

<br>

```bash
cd /opt/ros/humble
ls
```

<br>

![Check installation](/assets/img/2025-03-25/check-installation.png)


&nbsp;

---