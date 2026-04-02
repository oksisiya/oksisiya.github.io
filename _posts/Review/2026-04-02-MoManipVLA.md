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

이동 조작(mobile manipulation)은 일상생활 속 다양한 작업과 환경에서 인간을 지원하는 로봇 공학의 근본적인 과제이다. 하지만 기존의 이동 조작 방식은 대규모 훈련의 부족으로 인해 다양한 작업과 환경에 걸쳐 일반화하는 데 종종 어려움을 겪는다. 반면 최근의 시각-언어-행동(VLA) 모델의 발전은 인상적인 일반화 능력을 보여주었지만, 이러한 파운데이션(foundation) 모델은 고정 베이스(fixed-base) 조작 작업을 위해 개발되었다.

따라서 본 논문에서는 고정 베이스 조작에 대해 사전 학습된 VLA 모델을 이동 조작으로 전이해(transfer), 이동 조작 정책에 대해 작업과 환경에 걸쳐 높은 일반화 능력을 달성할 수 있게 하는 효율적인 <u>정책 적응 프레임워크(policy adaptation framework)인 MoManipVLA</u>를 제안한다.

* 사전 학습된 VLA 모델을 활용해 높은 일반화 능력을 갖춘 엔드 이펙터의 웨이포인트를 생성한다.

* 이동 베이스와 로봇 팔의 <u>움직임 계획 목표(motion planning objectives)를 설계</u>해 궤적의 물리적 실현 가능성(physical feasibility of the trajectory)을 극대화한다.

* 궤적 생성을 위한 효율적인 2단계 목표 최적화 프레임워크(bi-level objective optimization framework)를 제안한다.
    * (Upper-level optimization) 매니퓰레이터 정책 공간을 확장하기 위해 <u>베이스 움직임을 위한 웨이포인트를 예측</u>한다.
    * (Lower-level optimization) 조작 작업을 완료하기 위해 <u>최적의 엔드 이펙터 궤적을 선택</u>한다.

이러한 방식으로, MoManipVLA는 로봇 베이스의 위치를 제로샷(zero-shot) 방식으로 조정할 수 있으므로, 고정 베이스 VLA 모델로부터 예측된 웨이포인트를 실현 가능하게 만든다. OVMM 및 실세계에서 이뤄진 광범위한 실험 결과는 MoManipVLA가 <span style="background-color:#DCFFE4">최첨단(state-of-the-art) 이동 조작</span>보다 4.2% 더 높은 성공률을 달성하고, 사전 학습된 VLA 모델의 강력한 일반화 능력 덕분에 실세계 배포에는 <span style="background-color:#DCFFE4">50 훈련 비용(50 training cost)</span>만이 필요함을 보였다.  

<br>

## Self Q&A

<br>

**'50 training cost for real world deployment'에서 '50'는 무슨 말(어느 정도)?**

<br>

## Paper Link

<https://arxiv.org/pdf/2503.13446>