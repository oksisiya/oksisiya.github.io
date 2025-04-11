---
title: "[SLAM] Differential Drive Robot and Encoder"
date: 2025-04-08 14:31:00 +0900
categories: SLAM
---

&nbsp;

## 1. Differential Drvie Robot

<br>

차동 구동 로봇(differential drive robot)은 독립적인 두 개의 바퀴로 구동되는 로봇이다. 우리가 접하는 대부분의 이동 로봇(mobile robot)은 차동 구동 로봇이다. 모터의 구동 방식에 따라서 로봇의 움직임을 세 가지 유형으로 나타낼 수 있다.

<br>

|로봇의 상태|모터의 구동 방식|
|---|---|
|직선(전/후진) 이동|두 개의 모터가 같은 방향과 속도로 회전|
|회전|한 개의 모터는 정지하거나 다른 모터와 반대 방향으로 회전|
|곡선 이동|두 개의 모터가 같은 방향이지만 다른 속도로 회전|

<br>

아래 그림 [[1]](https://www.youtube.com/watch?v=LrsTBWf6Wsc)은 차동 구동 로봇의 모델링을 나타낸 것이다. 바퀴의 지름은 로봇의 이동 거리에, 바퀴와 바퀴 사이의 거리는 회전량에 영향을 미친다.

<br>

![Differential drive modeling](/assets/img/2025-04-08/differential-drive-modeling.png)

<br>

따라서 바퀴의 지름과 바퀴와 바퀴 사이의 거리를 알면 차동 구동 로봇의 오도메트리(odometry)를 계산할 수 있다.

<br>

## 2. Encoder

<br>

차동 구동 로봇의 움직임을 추정하려면 모터로부터 피드백을 받아야 한다. 이때 엔코더(encorder)를 사용한다. 모터의 펄스 신호를 통해 모터의 회전 방향은 제어할 수 있지만 회전량을 파악할 수는 없다. 엔코더를 모터에 부착해 사용하면 모터의 회전량을 구할 수 있다.

<br>

아래 그림 [[2]](https://anaheimautomation.com/blog/post/encoders-guide)은 엔코더의 구조를 나타낸 것이다.

<br>

![Encorder components](/assets/img/2025-04-08/encoder-components.png)

<br>

아래 그림[[3]](https://instrumentationtools.com/encoder-working-principle/)처럼 LED에서 방출된 빛은 포토 센서(photo sensor)에 의해 감지된다. 디스크(disk)가 회전해 포토 센서가 빛을 감지할 때마다 틱(tick)의 개수가 증가한다. 누적된 틱의 개수를 통해 바퀴가 얼마큼 회전했는지 알 수 있다.

<br>

![Encorder working principle](/assets/img/2025-04-08/encorder-working-principle.gif)

<br>

엔코더의 단위는 PPR(pulses per revolution)로, 분해능(resolution)이라고도 한다. 디스크가 한 바퀴 회전할 때 몇 개의 펄스(pulse)를 셀 수 있는지를 나타낸다. 예를 들어, 어떤 엔코더의 사양 중 분해능이 334 [PPR]이라면 엔코더의 디스크에 334개의 구멍이 뚫려있다는 것을 의미한다. 분해능이 높을수록 감지할 수 있는 위치가 많아 정밀한 제어가 가능하고, 분해능이 낮을수록 러프한 제어가 가능하다 [[4]](https://minji11203.tistory.com/29).

<br>

---

## References

[1] <https://www.youtube.com/watch?v=LrsTBWf6Wsc>  
[2] <https://anaheimautomation.com/blog/post/encoders-guide>  
[3] <https://instrumentationtools.com/encoder-working-principle/>  
[4] <https://minji11203.tistory.com/29>

&nbsp;