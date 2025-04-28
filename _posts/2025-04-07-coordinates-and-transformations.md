---
title: "[SLAM] Coordinates and Transformations"
date: 2025-04-07 17:38:00 +0900
categories: SLAM
use_math: true
---

&nbsp;

로봇 시스템은 아래의 그림 [[1]](https://motion.cs.illinois.edu/RoboticSystems/CoordinateTransformations.html)과 같이 다양한 연결과 센서에 대한 여러 개의 좌표 프레임(coordinate frames)으로 구성된다. 따라서 로봇 시스템을 다루기 위해서는 좌표(coordinates) 및 변환(transformations)에 대한 이해가 필요하다.

<br>

![Robot frames example](/assets/img/2025-04-07/willow-garage-pr2-robot.png)

<br>

## 1. Coordinates

<br>

### 1.1 2D Coordinates Frames

<br>

2D 공간 상에서 위치(position) $P$는 원점 $O$로부터 수평 거리 $p_x$만큼, 수직 거리 $p_y$만큼에 위치한 벡터 $\textbf{p}$로 표현된다.

<br>

$$\textbf{p}=(p_x, p_y)$$

<br>

로봇 시스템의 2D 좌표는 우리에게 익숙한 월드 좌표(world coordinate)와 차이가 있다. 로봇(로컬) 시스템에서는 로봇이 기준이 되어 로봇이 바라보는 방향을 x축, 그에 수직인 방향을 y축으로 삼는다.

<br>

|Coordinates|Axis|
|---|---|
|World|x축: 오른쪽, y: 위쪽|
|Local|x: 앞, y: 왼쪽|

<br>

![2D coordinates frame](/assets/img/2025-04-07/2d-coordinates-frame.png)|![2D robot](/assets/img/2025-04-07/robot-2d.png)

<br>

### 1.2 3D Coordinates Frames

<br>

3차원 공간 상에서 위치 $P$는 3개의 숫자로 이루어진 벡터 $\textbf{p}$로 표현된다.

<br>

$$\textbf{p}=\begin{bmatrix}
p_x \\
p_y \\
p_z
\end{bmatrix}$$

<br>

![3D coordinates frame](/assets/img/2025-04-07/3d-coordinates-frame.png)

<br>

## 2. Homogeneous Coordinates

<br>

동차 좌표(homogeneous coordinates)는 강체 변환(rigid transforms)을 확장된 공간에서 선형 변환(linear transformation)으로 표현한다.

<br>

동차 좌표의 핵심은 주어진 좌표에 추가적인 동차 좌표(additional homogeneous coordinate)를 더함으로써 점(위치, positional)과 벡터(방향, directional)를 구분한다는 것이다.

<br>

|Quantities|Point|Homogeneous Coordinate|
|---|---|---|
|Point|$p=\left[ x, y, z \right]^T$|$\hat{p}=\left[ x, y, z, 1 \right]^T$|
|Vector|$p=\left[ x, y, z \right]^T$|$\hat{v}=\left[ x, y, z, 0 \right]^T$|

<br>

또한, 동차 좌표는 좌표의 변환, 즉 이동(translation)과 회전(rotation)을 하나의 행렬로 나타낸다. 3D 공간 상에서 변환 행렬 $T$는 다음과 같이 4×4 행렬로 표현된다.

<br>

$$T=\begin{bmatrix}
r_{11} & r_{12} & r_{13} & t_x \\
r_{21} & r_{22} & r_{23} & t_y \\
r_{31} & r_{32} & r_{33} & t_z \\
0 & 0 & 0 & 1
\end{bmatrix}=\begin{bmatrix}
\textbf{R} & \textbf{t} \\
\textbf{0}^T & 1\
\end{bmatrix}$$

<br>

뿐만 아니라, 무한대에 있는 점을 표현할 수 있고 원근 투영을 선형 변환으로 나타낼 수 있다는 점에서 동차 좌표는 로보틱스와 컴퓨터 그래픽스 분야에서 널리 사용된다.

<br>

### 2.1 Translation

<br>

좌표의 이동은 소문자 $\textbf{t}$로 표현하며 3×1 행렬이다. 이동 행렬은 주어진 좌표를 일정 거리만큼 평행 이동시킨다. 3차원 공간 상에서 이동 행렬은 다음과 같다.

<br>

$$\textbf{t}=\begin{bmatrix}
t_x \\
t_y \\
t_z
\end{bmatrix}$$

<br>

### 2.2 Rotation

<br>

좌표의 회전은 대문자 $\textbf{R}$로 표현하며 3×3 행렬이다. 회전 행렬은 주어진 좌표의 방향을 바꾼다. 3차원 공간 상에서 각 축에 대해 $\theta$만큼 회전할 때 회전 행렬은 다음과 같다.

<br>

$$
R_x\left(\theta  \right)=\begin{bmatrix}
 1 & 0 & 0 \\
 0 & \cos\left( \theta \right) & -\sin\left( \theta \right) \\
 0 & \sin\left( \theta \right) & \cos\left( \theta \right)
\end{bmatrix}
$$

$$
R_y\left(\theta  \right)=\begin{bmatrix}
 \cos\left( \theta \right) & 0 & \sin\left( \theta \right) \\
 0 & 1 & 0 \\
 -\sin\left( \theta \right) & 0 & \cos\left( \theta \right)
\end{bmatrix}
$$

$$
R_z\left(\theta  \right)=\begin{bmatrix}
 \cos\left( \theta \right) & -\sin\left( \theta \right) & 0 \\
 \sin\left( \theta \right) & \cos\left( \theta \right) & 0 \\
 0 & 0 & 1
\end{bmatrix}
$$

<br>

주어진 좌표에 대해 회전 변환을 적용할 때 회전 순서(어느 축으로 먼저 회전하는지)에 따라서 변환 결과가 달라진다.

<br>

### 2.3 Combined Transformations

<br>

이동 행렬과 회전 행렬을 합쳐 하나의 변환 행렬 $T$를 구한다. 점 $\textbf{p}$의 변환을 다음과 같이 나타낼 수 있다.

<br>

$$\hat{T}\left( \hat{p} \right)=\begin{bmatrix}
\textbf{R} & \textbf{t} \\
\textbf{0}^T & 1\
\end{bmatrix}\cdot \hat{p}=\begin{bmatrix}
r_{11} & r_{12} & r_{13} & t_x \\
r_{21} & r_{22} & r_{23} & t_y \\
r_{31} & r_{32} & r_{33} & t_z \\
0 & 0 & 0 & 1
\end{bmatrix}\begin{bmatrix}
x \\
y \\
z \\
1
\end{bmatrix}=\begin{bmatrix}
x' \\
y' \\
z' \\
1
\end{bmatrix}$$

<br>

## 3. tf

<br>

하나의 로봇 시스템에서 로봇의 구성 요소(e.g., 바퀴, 센서 등)들은 각기 다른 위치와 방향을 가지며 이를 일관성 있게 연결하는 것이 필요하다. tf는 프레임(frame) 사이의 좌표 변환을 추적하고 관리하는 패키지이다 [[3]](http://wiki.ros.org/tf). tf는 SLAM과 Navigation을 수행할 때 중요한 패키지 중 하나이다.

<br>

예를 들어, 로봇에는 두 개의 프레임이 있다. `base_link`는 로봇의 중앙이 기준인 프레임이고 `base_laser`는 레이저의 중앙이 기준인 프레임이다 [[4]](https://wiki.ros.org/navigation/Tutorials/RobotSetup/TF). 우리는 수집한 센서 데이터를 `map`이나 `odom`, `base_link`와 같은 다른 프레임을 기준으로 변환해서 사용해야 한다.

<br>

![Transform configuration](/assets/img/2025-04-07/transform-configuration-1.png)

<br>

이때 tf 패키지는 `base_laser` → `base_link` → `odom` → `map`과 같이 좌표계 사이의 연결을 따라 데이터의 좌표를 자동으로 변환한다.

<br>

![Transform configuration](/assets/img/2025-04-07/transform-configuration-2.png)

<br>

---

## References

[1] <https://motion.cs.illinois.edu/RoboticSystems/CoordinateTransformations.html>  
[2] <https://ko.wikipedia.org/wiki/3%EC%B0%A8%EC%9B%90#/media/%ED%8C%8C%EC%9D%BC:Coord_planes_color.svg>  
[3] <https://wiki.ros.org/tf>  
[4] <https://wiki.ros.org/navigation/Tutorials/RobotSetup/TF>

&nbsp;