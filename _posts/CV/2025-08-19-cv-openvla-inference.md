---
title: "[VLA] OpenVLA 모델 추론 2) OpenVLA"
date: 2025-08-19 09:45:00 +0900
categories: [CV, Multi Modal]
---

&nbsp;


<br>

Hugging Face(🤗)의 openvla 페이지에는 총 일곱 개의 모델이 공개가 되어있다. 그중에서 이번 포스팅에서 사용할 모델은 openvla/openvla-7b 모델이다.

<br>

![Hugging Face OpenVLA](/assets/img/2025-08-19/Hugging-Face-OpenVLA.png)|![OpenVLA-7B](/assets/img/2025-08-19/OpenVLA-7B.png)

<br>
필요한 라이브러리
pip install timm (1.0.19 에서 다운그레이드)
pip install flash-attn --no-build-isolation
pip install timm==0.9.12

<br>

---

## References
[1]