---
title: "DexDiffuser: Generating Dexterous Grasps with Diffusion Models (6 Nov 2024)"
date: 2025-11-25 15:39:00 +0900
categories: [Review]
use_math: true
---
---

&nbsp;

- [ ] diffusion에서 guided = condition을 넣는 것. (어떤 데이터를 생성할지 control 못하기 때문에) / 로봇 제어 분야에서도 사용하려면 condition을 넣어주어야 e.g., 태스크 or 제약조건 등

- [ ] diffusion model로 학습하려면 경로 정보(<- RL로 학습시켜서 찾는 것 같은데...) 필요...

- [ ] 논문에서 motion conditioning은 뭔지

- [ ] Dexgen에서 diffusion model의 세팅(입력, 출력 등) 알아볼 것

- [ ] 논문에서 assist 의미가 어떤 건지? 어떻게 보조하는지?

- [ ] 각 task에 mode conditioning이 어떻게 들어가는지

- [ ] DexGen을 단일로 사용할 수 있는지 or just 보조 수단인지

- [ ] 보조하기 위해서 특별히 제안한 방법이 뭔지

- [ ] Distance function 어떤 의미인지. 학습에서 어떻게 활용되는지

- [ ] 학습 당시에는 보조 어떻게?

- [ ] Teleoperation을 dexgen이 어떻게 보조? (자유도 높 & 무선 장비 => 사람의 행동이 잘 반영되지 않을 수 있 / 위치로? noise 낄 수밖에. / 2 finger (모터) 와 비교 / 그런 거를 보조하려고 하는 건지..)

- [ ] 논문의 최종적 목표가 뭔지? & 구현 시 테크닉이 뭔지?

1) 논문 다시 읽기
2) Diffusion 공부
3) Policy 구현 해보기