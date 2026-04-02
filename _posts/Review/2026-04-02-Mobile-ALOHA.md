---
title: "[Review] Mobile ALOHA: Learning Bimanual Mobile Manipulation with Low-Cost Whole-Body Teleoperation"
date: 2026-04-02 15:50:00 +0900
categories: [Review]

published: true
use_math: true
---
---

<br>

## Abstract

<br>

인간 시연으로부터의 모방학습은 로봇 공학 분야에서 인상적인 성능을 보여왔다. 그러나 대부분의 결과는 탁상 위 조작(table-top manipulation)에 초점을 맞추고 있어, 일반적으로 유용한 작업에 필요한 이동성(mobility)과 섬세함(dexterity)이 부족하다. 본 논문에서는 양손(bimanual)을 사용하고 전신(whole-body) 제어가 필요한 이동 조작 작업을 모방하기 위한 시스템을 개발한다.

Mobile ALOHA는 <u>데이터 수집을 위한 저비용 전신 원격 조작(low-cost and whole-body teleoperation) 시스템</u>이다. 이것은 이동 베이스(mobile base)와 전신 원격 조작 인터페이스를 통해 ALOHA 시스템을 강화한다(augment). Mobile ALOHA로 수집한 데이터를 사용해 지도 행동 복제(supervised behavior cloning)를 수행하고, 기존의 정적인(static) ALOHA 데이터셋과의 공동 학습(co-training)이 이동 조작 작업에 대한 성능을 향상시킨다는 것을 발견한다.

각 작업에 대한 50번의 시연(demonstrations)을 통해, 공동 학습은 성공률을 최대 90%까지 증가시킬 수 있으며, 이를 통해 Mobile ALOHA는 새우를 볶고 서빙하기, 무거운 냄비를 보관하기 위해 두 문이 달린 벽장을 열기, 엘리베이터를 호출하고 탑승하기, 부엌 수도꼭지를 사용해 사용한 냄비를 가볍게 헹구기 등과 같은 복잡한 이동 조작 작업들을 자율적으로 완료할 수 있다.

<br>

## Self Q&A

<br>

## Paper Link

<https://arxiv.org/pdf/2401.02117>