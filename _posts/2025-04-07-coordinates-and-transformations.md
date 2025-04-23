---
title: "[SLAM] Coordinates and Transformations (yet)"
date: 2025-04-07 17:38:00 +0900
categories: SLAM
use_math: true
---

<br>

To be continued...

<br>

## 1. Coordinates

<br>

### 1.1 2D Coordinates Frames

<br>

![2D coordinates frame](/assets/img/2025-04-07/2d-coordinates-frame.png)

<br>

### 1.2 3D Coordinates Frames

<br>

![3D coordinates frame](/assets/img/2025-04-07/3d-coordinates-frame.png)

<br>

## 2. Transformations

<br>

### 2.1 Homogeneous Coordinates

<br>

$$
\text{w}\neq 0 \quad P=\begin{bmatrix}
x \\
y \\
z \\
1
\end{bmatrix},\quad \text{w}=0 \quad \text{V}=\begin{bmatrix}
x \\
y \\
z \\
0
\end{bmatrix}
$$

<br>

### 2.2 Rotation

<br>

$$
R_x\left(\alpha  \right)=\begin{bmatrix}
 1 & 0 & 0 & 0 \\
 0 & \cos\left( \alpha \right) & -\sin\left( \alpha \right) & 0 \\
 0 & \sin\left( \alpha \right) & \cos\left( \alpha \right) & 0 \\
 0 & 0 & 0 & 1
\end{bmatrix}
$$

$$
R_y\left(\beta  \right)=\begin{bmatrix}
 \cos\left( \beta \right) & 0 & \sin\left( \beta \right) & 0 \\
 0 & 1 & 0 & 0 \\
 -\sin\left( \beta \right) & 0 & \cos\left( \beta \right) & 0 \\
 0 & 0 & 0 & 1
\end{bmatrix}
$$

$$
R_z\left(\gamma  \right)=\begin{bmatrix}
 \cos\left( \gamma \right) & -\sin\left( \gamma \right) & 0 & 0 \\
 \sin\left( \gamma \right) & \cos\left( \gamma \right) & 0 & 0 \\
 0 & 0 & 1 & 0 \\
 0 & 0 & 0 & 1
\end{bmatrix}
$$

<br>

### 2.3 Translation

<br>

$$
T=\begin{bmatrix}
 1 & 0 & 0 & t_x \\
 0 & 1 & 0 & t_y \\
 0 & 0 & 1 & t_z \\
 0 & 0 & 0 & 1
\end{bmatrix}
$$

<br>

### 2.4 Combined Transformations

<br>

$$
T=\begin{bmatrix}
R & t \\
0 & 1
\end{bmatrix}
=\begin{bmatrix}
R_{3\times 3} & \begin{bmatrix}
t_x \\
t_y \\
t_z
\end{bmatrix} \\
0 & 1
\end{bmatrix}
=\begin{bmatrix}
r_{11} & r_{12} & r_{13} & t_x \\
r_{21} & r_{22} & r_{23} & t_y \\
r_{31} & r_{32} & r_{33} & t_z \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

<br>

$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1
\end{bmatrix}
=\begin{bmatrix}
r_{11} & r_{12} & r_{13} & t_x \\
r_{21} & r_{22} & r_{23} & t_y \\
r_{31} & r_{32} & r_{33} & t_z \\
0 & 0 & 0 & 1
\end{bmatrix}\begin{bmatrix}
x \\
y \\
z \\
1
\end{bmatrix}
$$

<br>

## 3. tf

<br>

하나의 로봇 시스템에서 로봇의 구성 요소(e.g., 바퀴, 센서 등)들은 각기 다른 위치와 방향을 가지며 이를 일관성 있게 연결하는 것이 필요하다. tf는 로봇의 frame들 사이의 좌표 변환을 추적하고 관리하는 패키지이다. 로봇의 각 부분의 위치 관계를 시간에 따라 관리하고, 서로 다른 좌표계 간의 변환을 쉽게 해준다.

<br>

예를 들어, 로봇에 라이다가 달려 있어 로봇이 `base_link`라는 좌표계와 `base_laser`라는 좌표계를 갖고 있다고 하자. 이때 라이다로부터 들어온 센서 데이터는 `base_laser` 좌표계 기준이다. 하지만 우리는 이 데이터를 `map`이나 `odom`, `base_link`와 같은 다른 좌표계 기준으로 변환해서 써야 할 때가 많다.

<br>

![Transform configuration](/assets/img/2025-04-07/transform-configuration-1.png)

<br>

이때 tf 패키지는 `base_laser` → `base_link` → `odom` → `map` 등과 같이 좌표계 간 연결을 따라 자동으로 변환해준다.

<br>

![Transform configuration](/assets/img/2025-04-07/transform-configuration-2.png)

<br>

SLAM과 Navigation에서 중요한 패키지이다.

<br>

---

## References

[1] https://motion.cs.illinois.edu/RoboticSystems/CoordinateTransformations.html

[2] https://ko.wikipedia.org/wiki/3%EC%B0%A8%EC%9B%90#/media/%ED%8C%8C%EC%9D%BC:Coord_planes_color.svg

http://wiki.ros.org/tf

https://wiki.ros.org/navigation/Tutorials/RobotSetup/TF (Transform configuration)

&nbsp;