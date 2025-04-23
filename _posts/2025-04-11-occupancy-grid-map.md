---
title: "[SLAM][Mapping] Occupancy Grid Map"
date: 2025-04-11 13:37:00 +0900
categories: SLAM
use_math: true
---

&nbsp;

대부분의 이동 로봇(mobile robot)은 원가 및 성능을 고려해 2D 라이더를 기반으로 SLAM 알고리즘을 수행한다.

<br>

미지의 환경에 대한 지도를 생성하는 매핑(mapping) 과정을 통해 생성되는 지도는 점유 격자 지도(occupancy grid map)이다. 아래 그림 [[1]](https://www.researchgate.net/figure/  Occupancy-grid-map-produced-during-a-multiple-robot-multiple-entry-trial-two-robots_fig2_2873269)은 점유 격자 지도의 예시이다.

<br>

![Occupancy grid map](/assets/img/2025-04-11/occupancy-grid-map-1.png)

<br>

점유 격자 지도는 공간을 셀(cell)로 나누어 표현한다. 각각의 셀은 센서 데이터를 바탕으로 셀의 상태를 나타내는 확률 값을 갖는다. 셀은 점유되거나(occupied) 비어 있을(free) 수 있다. 아래 그림 [[2]](https://journals.sagepub.com/doi/abs/10.1177/0278364918775523), [[3]](https://jinyongjeong.github.io/2017/02/21/lec10_Grid_map/)은 이를 나타낸다.

<br>

![Occupancy grid map](/assets/img/2025-04-11/occupancy-grid-map-2.png)|![Occupancy grid map](/assets/img/2025-04-11/occupancy-grid-map-3.png)

<br>

즉, 점유 격자 지도에서 각각의 셀은 장애물 여부에 대한 이진 확률 변수(binary random variable)이다.

<br>

|Cell의 상태|확률 값|
|---|---|
|Occupied|$p\left( m_i \right)=1$|
|Free|$p\left( m_i \right)=0$|
|Unknown|$p\left( m_i \right)=0.5$|

<br>

생성된 지도는 로봇의 현재 위치를 지도 상에서 추정하는 위치 추정(localization)이나 목적지까지 최적의 경로를 찾는 경로 계획(path planning)에 사용한다.

<br>

---

## References

[1] <https://www.researchgate.net/figure/  Occupancy-grid-map-produced-during-a-multiple-robot-multiple-entry-trial-two-robots_fig2_2873269>  
[2] <https://journals.sagepub.com/doi/abs/10.1177/0278364918775523>  
[3] <https://jinyongjeong.github.io/2017/02/21/lec10_Grid_map/>

&nbsp;