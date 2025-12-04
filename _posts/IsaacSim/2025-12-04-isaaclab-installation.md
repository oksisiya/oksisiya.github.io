---
title: "Isaac Lab 설치 (Isaac Sim pip 패키지 사용)(Recommended)"
date: 2025-12-04 12:26:00 +0900
categories: [IsaacSim]
---

---

&nbsp;

Isaac Lab은 Isaac Sim을 기반으로 로봇 연구(RL, learning from demonstrations, motion planning 등)에 최적화된 시뮬레이션 플랫폼이다. 특히 강화학습에 최적화된 구조와 도구들을 제공해 복잡한 로봇 학습을 효율적으로 수행할 수 있게 한다.

<br>

## **Installing Isaac Sim**

<br>

Isaac Sim을 설치하는 방법은 크게 두 가지가 있다.

* 압축 파일 다운로드/해제 ([관련 링크](<https://docs.isaacsim.omniverse.nvidia.com/5.0.0/installation/download.html>))
    * Isaac Sim이 특정 폴더에 독립적으로 설치되며 일반적인 애플리케이션처럼 쉽게 실행한다.
    * 시뮬레이터를 실행하는 전용 스크립트(`isaac-sim.sh`)를 사용한다.
    * Isaac Sim을 처음 접하거나 GUI 환경이 익숙한 사람에게 적합하다.

* Python 환경에 패키지 형태로 다운로드 ([관련 링크](<https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/pip_installation.html#installing-isaac-sim>)) (추천)
    * Isaac Sim API를 호출해서 코드를 통해 시뮬레이션을 제어한다.
    * GUI를 띄우려면 `from omni.isaac.kit import SimulationApp` 명령어를 실행해야 한다.
    * 특정 프로젝트에 필요한 Isaac Sim 버전을 환경별로 관리하기 용이하다.

<br>

처음 접했을 때는 Omniverse Launcher를 통해 Isaac Sim을 사용했고 최근에는 첫 번째 방법으로 설치해서 Isaac Sim을 사용하려고 했다. 시뮬레이터에서 제공하는 예제가 잘 실행되고 렌더링도 깔끔해서 문제가 없다고 생각했는데 어제 `. ~/isaac-sim/python.sh <파일 이름>` 명령어로 파일을 실행하려고 하니 어떠한 에러 메시지도 뜨지 않은 채 쉘이 종료되는 문제를 겪었다. 오늘 아침까지 원인을 찾다가 포기하고 두 번째 방법으로 다시 설치해서 사용 중인데 문제도 없고 첫 번째 방법보다 사용하기 더 깔끔한 것 같다. 논문마다 Python 버전이나 CUDA 버전이 다른데 각각에 맞는 Isaac Sim 버전을 맞추기도 편해서 두 번째 방법을 추천한다.

<br>

#### **Preparing a Python Environment**

<br>

* Create a virtual environment

```bash
# [Conda Environment]

conda create -n env_isaaclab python=3.11    # For Isaac Sim 5.X
conda activate env_isaaclab
```

<br>

* Ensure the latest pip version is installed

```bash
# [Linux]

pip install --upgrade pip
```

<br>

#### **Installing Dependencies**

<br>

* Install Isaac Sim pip packages

```bash
pip install "isaacsim[all,extscache]==5.1.0" --extra-index-url https://pypi.nvidia.com
```

<br>

* Install a CUDA-enabled PyTorch build that matches your system architecture

```bash
pip install -U torch==2.7.0 torchvision==0.22.0 --index-url https://download.pytorch.org/whl/cu128
```

<br>

#### **Verifying the Isaac Sim Installation**

<br>

* Check that the simulator runs as expected

```bash
isaacsim

# 최초 실행 시 Nvidia Omniverse 라이선스에 동의하냐는 메시지가 표시되는데 예(yes)라고 답한다.
By installing or using Isaac Sim, I agree to the terms of NVIDIA OMNIVERSE LICENSE AGREEMENT (EULA)
in https://docs.isaacsim.omniverse.nvidia.com/latest/common/NVIDIA_Omniverse_License_Agreement.html

Do you accept the EULA? (Yes/No): Yes
```
<br>

Window > Examples > Robotics Examples > MANIPULATION > Follow Target 예제를 실행해 보면 

<br>

![Isaac Sim](/assets/img/2025-12-04/isaac-sim.png)

<br>

## **Installing Isaac Lab**

<br>

#### **Cloning Isaac Lab**

<br>

* Clone the Isaac Lab repository

```bash
# [HTTPS]

git clone https://github.com/isaac-sim/IsaacLab.git
```

<br>

#### **Installation**

<br>

* Install dependencies using `apt` (on Linux only)

```bash
sudo apt install cmake build-essential
```

<br>

* Run the install command

```bash
# [Linux]

./isaaclab.sh --install
```

<br>

#### **Verifying the Isaac Lab Installation**

<br>

* Run the following command from the top of the repository

```bash
# [Linux]

./isaaclab.sh -p scripts/tutorials/00_sim/create_empty.py
```

<br>

시뮬레이션이 실행되고 검은색 뷰포트(viewport)가 있는 창이 표시되는 걸 확인할 수 있다.

<br>

![Isaac Lab](/assets/img/2025-12-04/isaac-lab.png)

<br>

`Ctrl + C`를 눌러 스크립트를 종료한다.

<br>

---

## Reference

<https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/pip_installation.html>