---
title: "[Review] MoManipVLA: Transferring Vision-language-action Models for General Mobile Manipulation"
date: 2026-04-02 09:32:00 +0900
categories: [Review]

published: true
use_math: true
---
---

<br>

## Abstract

<br>

이동 조작(mobile manipulation)은 일상생활 속 다양한 작업과 환경에서 인간을 지원하는 로봇 공학의 근본적인 과제이다. 하지만 기존의 이동 조작 방식은 대규모 훈련의 부족으로 인해 다양한 작업과 환경에 걸쳐 일반화하는 데 종종 어려움을 겪는다. 반면 최근의 시각-언어-행동(VLA) 모델의 발전은 인상적인 일반화 능력을 보여주었지만, 이러한 파운데이션 모델은 고정 베이스(fixed-base) 조작 작업을 위해 개발되었다.

따라서 본 논문에서는 고정 베이스 조작에 대해 사전 학습된 VLA 모델을 이동 조작으로 전이해(transfer), 이동 조작 정책에 대해 작업과 환경에 걸쳐 높은 일반화 능력을 달성할 수 있게 하는 효율적인 <u><span style="background-color:#DCFFE4">정책 적응(policy adaptation)</span> 프레임워크</u>인 MoManipVLA를 제안한다.

* 사전 학습된 VLA 모델을 활용해 높은 일반화 능력을 갖춘 엔드 이펙터의 웨이포인트를 생성한다.

* 이동 베이스와 로봇 팔의 <u>움직임 계획 목표(motion planning objectives)를 설계</u>해 궤적의 물리적 실현 가능성(physical feasibility of the trajectory)을 극대화한다.

* 궤적 생성을 위한 효율적인 2단계 목표 최적화 프레임워크(bi-level objective optimization framework)를 제안한다.
    * (Upper-level optimization) 매니퓰레이터 정책 공간을 확장하기 위해 <u>베이스 움직임을 위한 웨이포인트를 예측</u>한다.
    * (Lower-level optimization) 조작 작업을 완료하기 위해 <u>최적의 엔드 이펙터 궤적을 선택</u>한다.

이러한 방식으로, MoManipVLA는 로봇 베이스의 위치를 제로샷(zero-shot) 방식으로 조정할 수 있으므로, 고정 베이스 VLA 모델로부터 예측된 웨이포인트를 실현 가능하게 만든다. OVMM 및 실세계에서 이뤄진 광범위한 실험 결과는 MoManipVLA가 <span style="background-color:#DCFFE4">최첨단(state-of-the-art) 이동 조작</span>보다 4.2% 더 높은 성공률을 달성하고, 사전 학습된 VLA 모델의 강력한 일반화 능력 덕분에 실세계 배포에는 <span style="background-color:#DCFFE4">50 훈련 비용(50 training cost)</span>만이 필요함을 보였다.  

<br>

## Self Q&A

<br>

**Policy adaptation이란?**

이미 학습된 모델을 새로운 상황이나 환경에 맞게 수정해 적용하는 기술. (Gemini 피셜)

* Transfers pre-trained VLA models for mobile manipulation (MoManipVLA, 2025)
* 다른 논문에서는...?

<br>

**이동 조작 분야의 SOTA 기술과 작업과 데이터셋**

Open Vocabulary Mobile Manipulation (OVMM) benchmark (2023)

* 실제 집의 구조를 근사화한 60개의 장면 모델과 18,000개 이상의 일상 사물 3D 모델을 포함

* Mobile manipulation task: "Move a target object from container A to container B"
    * 목표 물체는 Hello Robot Stretch 로봇이 잡을 수 있는 작은 물체이다.
    * 로봇은 알 수 없는 환경에서 초기화되며, "Nav to Recp-A, Gaze, Pick Object, Nav to Recp-B, and Place" 단계를 순차적으로 수행해야 한다. 이러한 단계 중 하나라도 오류가 발생하면 조작은 실패한다.

* Mobile manipulation expert trajectories
    * OVMM에서 제공하는 휴리스틱 기준선(heuristic baseline)을 사용해 전문가 궤적을 수집하고, 이를 VLA 모델(OpenVLA-7B)로 미세조정(fine-tune)해서 cross-embodied gaps을 해소한다.
    * 각 전문가 궤적은 시각적 인식(visual perception), 로봇 상태(robot states), 그리고 실행 동작(execution actions)을 포함하는 일련의 튜플로 구성된다.
    * 본 논문에서는 200개의 pick-and-place 데모 에피소드를 수집했다.
* Trajectory optimization framework
    이중 어닐링 기법(double annealing) 경유지(waypoints) 사이의 물리적으로 가능한 궤적을 찾기 위함.

<br>

**'50 training cost for real world deployment'에서 'training cost'의 의미**

전문가 시연.

* (Introduction) Due to the strong generalization of our pre-trained VLA models, MoManipVLA only requires <u>50 expert episodes</u> for real-world deployment.
* (4.4. Real World Experiment) Benefiting from the generalization of the pre-trained VLA models, our approach requires only 50 samples to complete the fine-tuning and achieves 40% success rate on the mobile manipulation task.
* 보통 몇 개 정도 필요한데?

<br>

## Paper Link

<https://arxiv.org/pdf/2503.13446>