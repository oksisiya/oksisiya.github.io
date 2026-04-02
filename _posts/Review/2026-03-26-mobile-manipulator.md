---
title: "논문 리뷰 목록"
date: 2026-03-26 13:41:00 +0900
categories: [Review]

published: true
use_math: true
---
---

<br>

## Mobile Manipulator

<br>

#### AC-DiT: Adaptive Coordination Diffusion Transformer for Mobile Manipulation

- NeurIPS (2025)
- <https://arxiv.org/pdf/2507.01961>
- 모바일 베이스의 이동이 로봇 팔 조작에 미치는 영향을 사전 정보로 반영하여 전신 제어의 오차를 줄인 Policy 모델. 모바일 로봇의 공간 이해를 위해 이미지 정보와 포인트 클라우드를 동적으로 융합하는 모듈 제안.

<br>

#### MoManipVLA: Transferring Vision-language-action Models for General Mobile Manipulation

- CVPR (2025)
- <https://arxiv.org/pdf/2503.13446>
- 고정 베이스 조작에 대한 사전 학습된 VLA 모델을 이동 조작으로 전이하여(fixed_base manipulation policy -> whole-body motion planning) 작업 및 환경에 대해 높은 일반화 성능을 달성하게 하는 정책 적응 프레임워크. 1) [VLA Manipulation Guidelines] 사전 학습된 VLA 모델로 최적의 엔드 이펙터 웨이포인트를 생성. 2) [Bi-level Trajectory Optimization] 엔드 이펙터가 목표 웨이포인트에 도달할 수 있도록 물리적 실현 가능성(도달 가능성, 부드러움, 안전성)이 가장 높은 베이스와 암의 궤적을 동시에 생성: (Upper-level) 베이스 이동 웨이포인트를 예측, (Lower-level) 최적의 엔드 이펙터 궤적을 선택.

<br>

#### Mobile ALOHA: Learning Bimanual Mobile Manipulation with Low-Cost Whole-Body Teleoperation

- CoRL (2024)
- <https://openreview.net/pdf?id=FO6tePGRZj>
- Mobile ALOHA는 저비용 전신 원격 제어 시스템으로, 기존 ALOHA에 이동형 베이스를 결합해 복잡한 양팔 작업을 가능하게 함. 50회의 시연 데이터와 기존 데이터의 공동 학습을 통해 새우 요리, 엘리베이터 탑승 등 고난도 이동 조작 과제에서 최대 90%의 성공률을 달성.