---
title: "SuperPoint"
date: 2025-05-21 09:30:00 +0900
categories: CV
---

&nbsp;

<br>

슈퍼포이느는 AI를 기반으로 특징을 뽑는. 특징 키포인트를 뽑는 그런 키포인트 익스트랙션 방법 중 가장 유명한 방법 중 하나이다.

<br>

## 1. Introduction

<br>

* Fully convolutional network 구조를 활용한 image feature extraction
* Forward pass 한 번으로 interest points와 이에 상응하는 descriptors 획득
* synthetic data 기반의 Pre-training
* Homographic adaptation 기술 적용
* Titan X GPU 기준 70fps(13ms)의 속도로 동작

<br>

가장 기본적으로 SuperPoint라고 하는 방법이 어떤 식으로 접근하고 있는지 먼저 살펴본다. 이 방법은 딥러닝으로 이미지 키포인트를 뽑는 방법 중에 가장. 이게 나왔던 당시에는 유명한 방법 중 하나였구요. 그리고 이 방법을 딥러닝을 이용해서 하기 위해서 fully convolutional network 구조를 활용해가지고 이미지 feature를 뽑는 그런 방법론을 제안한 방법이 Superpoint가 된다. 이 방법을 통해서 forward pass 한 번만 지나가는데. 그 한번 지나가는 것만으로도 interest point. 키포인트에 해당하는 포인트랑 그리고 이거에 상응하는. 키포인트 디스크립터를 취득할 수 있었다. 라는 게 장점이라고 한다. 그리고 이거에 대한 네트워크를 학습하기 위해서 신데딕 데이터를 이용을 하였고. 그 신데딕 데이터를 이용해서 프리 트레이닝을 충분히 한 다음에 이미지 피쳐 익스트렉션을 잘 수행할 수 있는 네트워크를 만들었다. 라고 하고 있다. 추가적으로 여기는 homographic adaptation 이라는 기술이 들어갔는데. 그 부분도 같이 뒤에서 설명하도록 한다. 이제 결과적으로는 Titan X GPU를 기준으로 해서 70fps. 그니까 13ms 정도의 속도로 feature extraction을 동작시킬 수 있었다. 라고 하고 있는데. 요게 지금 요새 나온 GPU 같은 걸로 돌렸을 때도 꽤나 이제 빠른 속도로 그 feature를 뽑을 수 있다. 라는 게 이제 장점이라고 볼 수 있다.

<br>

## 2. SuperPoint Architecture

<br>

![Self-Supervised-Training Overview](/assets/img/2025-05-21/self-supervised-training-overview.png)

<br>

구조를 전반적으로 보면. 구조 같은 경우는 크게 세 개로 나눌 수 있다. 첫 번째는 interest Point에 대한 pre training 부분. 그리고 self labeling(레이블링). 그리고 joint training.

<br>

### 2.1 Shared Encoder

<br>

![SuperPoint Decoders](/assets/img/2025-05-21/superpoint-decoders.png)

<br>

전체적인 구조가 아래 나와있다. 아래 나와있는 구조 중 encoder 부분을 살펴보면. 이 인코더 구조 자체는 VGG 네트워크를 활용했다. 그래서 VGG를 기반으로 네 개의 3x3x64 레이어를 쌓고 그 다음에 네 개의 3x3x128 레이더. 이 위에 나와 있는 그림과 같이 네트워크를 구성했다고 볼 수가 있다. 그래서 이런 식으로 네트워크를 구성하고 활성화 함수에 렐루. 맥스풀링을 이제 사용했다라고 애기를 하고 있는데. 이렇게 인코더를 거쳐가지고 우리가 세 번의 풀링을 거쳤다고 볼 수 있는데. 결과적으로는. 그래서 이 세 번의 풀링을 하다 보니까 원래 원본보다는 8배 정도 축소된 피쳐맵이 나오게 된다. 그런 식으로 피쳐를 얻게 되고. 이 출력된 피쳐 맵 같은 경우에는 어떤 하나의 셀로 구성되어 있다고 볼 수가 있는데. 이 셀 같은 경우에는 8x8 픽셀을 대표하는 하나의 값이라고 간주를 하고 있고. 이게 8배로 축소되어 있다 보니까 그러면 이제 그 8x8 크기의 어떤 영역에 대해서 하나의 픽셀 값으로. 특징 픽셀 값으로 축소되서 특징 맵으로 함축되어 있다라고 볼 수 있다. 그런 식으로 이제 CNN을 통해가지고 어떤 이제 이런 feature map을 만들어내고 있다라고 볼 수가 있다.

<br>

### 2.2 Interest Point Decoder

<br>

![SuperPoint Decoders](/assets/img/2025-05-21/superpoint-decoders.png)

<br>

### 2.3 Descriptor Decoder

<br>

![SuperPoint Decoders](/assets/img/2025-05-21/superpoint-decoders.png)

<br>

### 2.4 Loss Functions

<br>

## 3. Synthetic Pre-Training

<br>

### 3.1 Synthetic Shapes

<br>

![Synthetic Shapes Dataset](/assets/img/2025-05-21/synthetic-shapes-dataset.png)

<br>

### 3.2 MagicPoint

<br>

![Synthetic Pre-Training](/assets/img/2025-05-21/synthetic-pre-training.png)

<br>

## 4. Homographic Adaptation

<br>

![Homographic Adaptation](/assets/img/2025-05-21/homographic_adaptation.png)

<br>

### 4.1 Formulation

<br>

### 4.2 Choosing Homographies

<br>

![Random Homography Generation](/assets/img/2025-05-21/random-homography-generation.png)

<br>

### 4.3 Iterative Homographic Adaptation

<br>

![Iterative Homographic Adaptation](/assets/img/2025-05-21/iterative-homographic-adaptation.png)

<br>

레이블되지 않은 이미지와 base detector가 입력으로 들어간다. 랜덤하게. 이미지에 대해서 호모그라피. 그니까 이제 워핑을 시킨다. 그래서 n 가지의 어떤 호모그라피. 매트릭스가 있다고 하면은. 이 호모그라피 매트릭스를 레이블되지 않은 이미지를 곱해가지고. 워핑된 이미지를 얻게 된다. 워핑된 이미지를 통해서 base detector를 거치게 되고. 거치게 된 결과에서 어떤 포인트 리스폰스가 나오게 됨. 이제 이 결과를 토대로 히트맵을 언워핑 시켜가지고 이런 식으로 결과를 얻게 되는. 그니까 이 언워핑 시킨다는 게 언레이블드. 오리지널 이미지랑 동일한 형태로 언워핑을 시킨다. 라고 볼 수가 있다. 그래서 1번 호모그래피에서 나온 이미지. 이미지랑 그 포인트 결과. 2번 n번에 나오는 그 포인트 결과들을 다 조합 시켜서 전체 히트맵을 얻을 수 있다. 이런 식으로 호모그래피를 통해서 데이터들 간 각각 학습을 하고서. 거기서 이제 aggregation 시킨 다음에 interest point superset을 얻는 게 여기서 이제 또 적용된 학습 방법이다.

<br>


<br>

<br>

### Random Homography Generation

<br>

그래서 이거를 위해서 random homography가 들어가게 된다. 이제 detector function 자체하고. homography는 이제 상관관계가. covariant 하지 않다. 라는 전제를 먼저 깔고 시작을 하고 있다. 그런데 이제 이 detector 함수하고. 아까 말한 네트워크하고 호모그래피. 이 두 가지가 완벽하게 covariant 하면 좋은데. 완벽하게 covariant 하지는 않았다. 라고 논문에서 이야기 하고 있다. 그래서 그런 부분을 잘 지적하는 게 세 번째 식이고. 요 세 번째 식이 의미하는 바가. 어떻게 보면 호모그래피를 적용하고 그 다음에 함수를 거치는. 그 다음에 언와핑하는 건데. 이 결과가(세 번째 결과가) 사실 1번 식하고 동일한 결과가 나와야 되는데. 항상 동일한 결과가 나오진 않았다. 라는 점을 지적하고 있다. 그래서 이거를 좀 해결하기 위해서 라지 N이라고 하는 충분히 큰 수(네 번째 식에서) 만큼의 랜덤 호모그래피 샘플을 얻어내고. 거기서 이제 평균을 얻어내는데. 그 평균을 얻어내면서 이제 우리가 랜덤 호모그래피를 진행한다고 얘기를 하고 있다. 그래서 요 N 같은 경우에는 이제 N 정도의 값에서 좋은 성능을 보였고. 이것보다 큰 값에서는 너무 커봤자 비슷한 성능만 내고 있었다는 결론을 도출할 수 있었다 라고 얘기를 하고 있다. 그래서 이런 방법을 통해서 최대한 이 입력하고(첫 번째 식) 동일한 결과가 나올 수 있게끔. 호모그래피를 찾자. 라는 게 목적이었고. 추가적으로 이제 (두 번째 그림에서) center crop이라든가. Crop 방법들이 여러 가지가 있는데. Homographic crop이라는 방법도 같이 이용해서(밑 그림에서 맨 마지막) 부분적인 호모그라피 적용하는 방법도 같이 사용을 하고 있었다.

<br>

### Iterative Homographic Adaptation

<br>

(요약) Training 과정에서 homographic adaptation을 반복적으로 적용하며 성능을 향상시킴

homographic adaptation 자체를 한번에 다 적용하는 게 아니라 iterative하게 적용하는 방법을?? 했다. training 과정에서 반복적으로 homography adaptation을 적용하면서 성능을 반복적으로 향상시키는 방법을 채택. 한꺼번에 하는 것보다는 점진적으로 성능이 좋아지는 강점을 보임.

<br>

## 5. Experiments

<br>

![Qualitative_Results_on_HPatches](/assets/img/2025-05-21/qualitative-results-on-hpatches.png)

<br>

실험 결과를 보면. Superpoint를 통해 상대적으로 많은 키포인트들을 골고루 추출해낸 것을 볼 수 있다. 조금 희미하지만 빨간색 포인트들이 interest ponit라고 볼 수가 있고. 이게 요게 지금 사실 두 개의 이미지가 있는데. 두 개의 이미지에서 매칭이 되고 있는. 그니까 이 매칭은 왼쪽 이미지에서 뽑은 포인트들과 오른쪽 이미지에서 뽑은 포인트 간의 매칭. 매칭이 잘 되고 있냐 라는 걸 볼 수가 있는 건데. 이제 superpoint에서 뽑은 특징들이 고르게 잘 뽑히고 매칭 정확도도 높다. 라는 걸 지금 보여주고 있다. 그거에 반해서 LiFT나 SIFT, ORB 같은 고전적인 방법들에서는 그런 이제 매칭되는 쌍들도 많지 않았고. 어떤 부분에 있어서는 superpoint가 좀 더 강인하게 매칭을 수행할 수 있지 않았을까. 라는 걸 보여주고 있었다. 다만 좀 단점이라고 하면 LIFT라고 하는 방법이. 회전이 이제 SIFT나 ORB에 비해선 회전에 강인하지 못했는데. 아무래도 learning 방법들이 회전. 이때까지는 회전된 사진에 대해서 강인하지 못했다 라는 한계점이 있었다고 한다. 아무래도 좀 cnn이 rotation에 조금 약한 부분이 있었다 보니까 그런 부분이 나온 것 같은데. sift나 orb는 좀 회전된 이미지에 대해서도 그래도 좀 특징을 잘 뽑는 반면. superpoint 같은 경우에는 15도에서 30도까지의 회전은 그래도 고려를 했는데. 그 이상에 대해서는 잘 뽑지 못하는. 문제점이 있었다 라고 얘기를 하고 있다. 아무래도 여기서 이제 구조가 아까 말씀드린 것처럼 cnn이 들어갔기 때문에. 그런 부분이 있다고 볼 수가 있고.

<br>

다음 결과를 봐도 이제 거기서 이제 회전에 대한 결과를 볼 수가 있는데. LIFT에 비해서는 회전에 대해서 잘 뽑았지만. 조금 더 풍부한 corresponding 매칭은 SIFT나 ORB에서 볼 수 있었다 라는 걸 알 수가 있다. 그래서 이제 그런 면에서는. 회전에 대해서는 아쉬운 부분이 조금 있었지만. 이제 딥러닝을 이용해서 이 정도로 이제 좋은 성능을 낼 수 있었다라는 거를 보여줄 수 있는. 그런 좀 의의가 있는 논문이지 않았을까...

<br>

---

## References

[1] <https://arxiv.org/pdf/1712.07629>

&nbsp;