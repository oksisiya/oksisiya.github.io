---
title: "Vision Language Action Model"
date: 2025-08-07 11:59:00 +0900
categories: [CV, Multi Modal]
---

&nbsp;

<br>

> ## **SayCan**
##### Do As I Can, Not As I Say: Grounding Language in Robotic Affordances
##### (<https://arxiv.org/abs/2204.01691>)

<br>

**Large Language Model (LLM)**
* Planning
* 540B PaLM

**Reinforcement Learning (RL)**
* Affordance



![SayCan Figure 1](/assets/img/2025-08-07/saycan-1.png)

<br>


![SayCan Figure 2](/assets/img/2025-08-07/saycan-2.png)

<br>



<br>

> ## **RT-1**
##### RT-1: Robotics Transformer for Real-World Control at Scale
##### (<https://arxiv.org/abs/2212.06817>)

<br>

**RT-1**
* SayCan + Imitation Learning

<br>

![RT-1 Figure 1](/assets/img/2025-08-07/rt1-1.png)

<br>

**Dataset**

13만 개의 에피소드로 구성된 700 개 이상의 작업

<br>

![RT-1 Figure 2](/assets/img/2025-08-07/rt1-2.png)

<br>

![RT-1 Figure 3](/assets/img/2025-08-07/rt1-3.png)

<br>


**Conclusion**
* 기존의 방법들보다 새로운 작업, 객체, 환경에 대해 효과적으로 일반화할 수 있다.
* 새로운 지시에 대한 일반화는 이전에 본 적 있는 개념들의 조합으로 제한되며 이전에 본 적 없는 완전히 새로운 동작으로 일반화 할 수 없다. (모방학습의 한계)

<br>

![RT-1 Figure 4](/assets/img/2025-08-07/rt1-4.png)

<br>

> ## **PaLM-E**
##### PaLM-E: An Embodied Multimodal Language Model
##### (<https://arxiv.org/abs/2303.03378>)

<br>

![PaLM-E Figure 1](/assets/img/2025-08-07/palme-1.png)

<br>

![PaLM-E Figure 2](/assets/img/2025-08-07/palme-2.png)

<br>

> ## **RT-2**
##### RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control
##### (<https://arxiv.org/abs/2307.15818>)

<br>

![RT-2 Figure 1](/assets/img/2025-08-07/rt2-1.png)

<br>

![RT-2 Figure 2](/assets/img/2025-08-07/rt2-2.png)

<br>

![RT-2 Figure 3](/assets/img/2025-08-07/rt2-3.png)

<br>

> ## **OpenVLA**
##### OpenVLA: An Open-Source Vision-Language-Action Model
##### (<https://arxiv.org/abs/2406.09246>)

<br>

![OpenVLA Figure 1](/assets/img/2025-08-07/openvla-1.png)

<br>

![OpenVLA Figure 2](/assets/img/2025-08-07/openvla-2.png)

<br>

**Dataset: Open X-Embodiment**
* <https://github.com/google-deepmind/open_x_embodiment>
* <https://colab.research.google.com/github/google-deepmind/open_x_embodiment/blob/main/colabs/Open_X_Embodiment_Datasets.ipynb>