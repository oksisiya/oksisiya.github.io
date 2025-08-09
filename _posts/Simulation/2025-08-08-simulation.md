---
title: "시뮬레이터 소개"
date: 2025-08-08 13:29:00 +0900
categories: [Simulation]
---

&nbsp;

이번 포스팅에서는 여러 가지 로봇 시뮬레이터(Simulator)에 대해 소개한다.

<br>

> ## **CoppeliaSim**
##### (<https://www.coppeliarobotics.com>)

<br>

* Coppelia Robotics
* 물리 엔진
    * 여러 물리 엔진 중 선택 가능
    * Bullet, Newton Game Dynamics, Open Dynamics Engine, Vortex Studio 등
* 렌더링
    * MuJoCo나 PyBullet 수준
    * 커스터마이징 가능
* 언어/API  
&nbsp;Lua(base), Python, C++, Java, MATLAB
* 활용 분야  
&nbsp;로봇 연구, 로봇 프로토타입 개발, 산업 자동화 시뮬레이션 등
* 특징
    * 분산 제어 아키텍처 지원
    * 다양한 내장 기능 및 모델
* 한계점
    * 상업용 라이선스: 유로 (학습용 라이선스: 무료)
    * 높은 학습 곡선

<br>

> ## **Drake**
##### (<https://drake.mit.edu>)

<br>

* Toyota Research Institute
* 물리 엔진  
&nbsp; &nbsp;자체 엔진을 사용하나 외부 엔진과 연동 가능
* 렌더링  
&nbsp; 기본적인(단순한) 시각화
* 언어/API  
&nbsp;C++ (Python 바인딩 가능)
* 활용 분야  
&nbsp;로봇 연구, 로봇 시스템 모델링, 모션 플래닝, 시스템 분석과 검증, 최적화 등
* 특징
    * 분석 정확도에 초점
    * 수학 기반의 고급 분석도 제공
    * 모델 기반의 접근법에 적합
* 한계점
    * GUI와 상호작용 또는 사실적인 렌더링의 어려움
    * 높은 학습 곡선

<br>

> ## **Gazebo**
##### (<https://gazebosim.org>)

<br>

* Open Robotics
* 물리 엔진
    * 여러 물리 엔진 중 선택 가능
    * Bullet, Open Dynamics Engine, Dynamic Animation and Robotics Toolkit 등
* 렌더링
    * OGRE (Object-Oriented Graphics Rendering Engine)
    * MuJoCo나 PyBullet보다는 우수하나 Isaac Sim 퀄리티는 아님
* 언어  
&nbsp;Python, C++
* 활용 분야  
&nbsp;ROS 기반의 로봇 시스템 개발 및 테스트, 센서 시뮬레이션 등
* 특징
    * ROS 특화. ROS 생태계와 완벽하게 통합될 수 있음
    * ROS 기반이다 보니 카메라, 라이다, imu 같은 여러 가지 센서에 대한 시뮬레이션 잘 지원
    * ROS 2 인터페이싱 굉장히 잘 되어 있음. 센서 모델에 대해서 풍부하게 지원을 하고 있고 개발 생태계가 잘 되어 있음. 확장성도 굉장히 넓음
    * 플러그인 아키텍처로 확장도 용이
* 한계점
    * 느린 프로그램 속도
    * 설정하고 사용하는 부분에 있어서 복잠함

<br>

> ## **Isaac Sim**
##### (<https://developer.nvidia.com/isaac/sim>)

<br>

* NVIDIA
* 물리 엔진  
&nbsp;PhysX 5
* 렌더링
    * 고품질의 렌더링 지원
    * 레이 트레이싱 지원
    * 레이 트레이싱. rtx 기술을 활용하는 초고품질의 사실적인 렌더링 지원
* 언어/API  
&nbsp;Python
* 활용 분야
    * 비전 분야에서의 인식 알고리즘 개발
    * 합성(synthetic) 데이터 생성
* 특징
    * 엔비디아 생태계(옴니버스)에서 개발된 툴 중 하나
    * ROS 2 지원 -> ROS 2와 연동한 디지털 트윈 구축 용이하게
    * 복잡한 시뮬레이션 환경 구성 가능
    * 현실도가 높은 사실적인 로봇 시스템 시뮬레이터
    * 엔비디아 GPU 적극적 사용 -> GPU 가속
* 한계점  
&nbsp;고사양의 GPU 필요 (엔비디아 GPU 일지라도 저사양의 GPU를 사용했을 때는 잘 돌아가지 않음)

<br>

> ## **MuJoCo**
##### (<https://mujoco.org>)

<br>

* Google DeepMind
* 물리 엔진  
&nbsp;자체 엔진 사용
* 렌더링  
&nbsp;기본적인(단순한) 시각화
* 언어/API  
&nbsp;
* 역학 계산에 특화. 물체하고 물체가 접촉하는 그런 역학을 계산하는.
* 속도 측면에서도 굉장히 시뮬레이터 치고 가벼움. 빠르고 안정적으로 물리 시뮬레이션을 제공할 수 있음. 그래서 가볍게 실습하기에 좋은 대상
* 강화학습 쪽에서 많은 연구.
* 멀티조인트의 어떤 CF? 그니까 MJ CF라고 하는 모델링과 urdf라고 하는 모델링을 지원. 여러 가지 확장 urdf를 활용해서 시뮬레이션을 구성할 수 있음.
* 로봇 제어, 동역학 연구, 강화학습, 생체역학
* 50 바인딩을 제공하고 있어서 Python api 사용 가능 c나 C++로도 확장 가능
오픈 소스에 대한 것도 있지만 강화학습에 대한 최적화도 되어 있고 물리 시뮬레이션에 대한 속도나 정확성 좋음.
* 단점으로는 단순한 렌더링. 현실적인 실사에 가까운 거나 복잡하고 화려한 렌더링에 적합하지 않음
* 단순한 센서는 다룰 수 있지만 복잡한 센서 시뮬레이션에 굉장히 기능이 제한적

<br>

> ## **PyBullet**
##### (<https://pybullet.org>)

<br>

* 물리 엔진  
&nbsp;Bullet
* 렌더링  
&nbsp;OpenGL의 기본적인 렌더링 지원
* 언어/API  
&nbsp;Python, C++
* 활용 분야  
&nbsp;로본 연구, 로봇 프로토타입 개발, 강화학습
* 특징
    * VR 지원
    * 다른 시뮬레이터에 비해 사용이 쉬운 API 제공
* 한계점
    * 접촉 모델에 대한 물리 정확성은 무조코와 비교하면 조금 떨어질 수 있음
    * 다소 단순한 렌더링 품질

<br>

> ## **Webots**
##### (<https://cyberbotics.com>)

<br>

* Cyberbotics
* 물리 엔진  
&nbsp;ODE 기반의 커스텀 엔진
* 렌더링  
&nbsp;Gazebo보다는 사실적인 표현
* 언어  
&nbsp;Python, C++, Java, MATLAB
* 활용 분야  
&nbsp;로봇 연구, 로봇 프로토타입 개발
* 특징
    * 오픈 소스
    * ROS 2 인터페이스 제공
    * 크로스 플랫폼 지원 (Windows, macOS, Linux)
    * 사용자 친화적인 GUI (코드를 보며 시뮬레이션 할 수 있음)
    * 문서화가 잘 돼있어 개발하기 편리
    * 다양한 로봇 모델 및 센서 라이브러리 내장
* 한계점
    * 다만 접촉 동역학의 성능이라든가 정확성에 대해서 차이가 있을 수 있음
    * 렌더링이 아이작심하고 비교할 수 있는 수준까지는 안 됨