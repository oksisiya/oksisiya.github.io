---
title: "[CLIP] Zero-Shot Image Classification"
date: 2025-07-17 09:57:00 +0900
categories: [CV, Multi Modal]
---

&nbsp;

이번 포스팅에서는 CLIP 모델을 사용해 Zero-Shot Classification을 수행하고 그 결과를 시각화한다.

<br>

## Model & Dataset

<br>

모델과 데이터셋은 Hugging Face(🤗)의 `openai/clip-vit-base-patch32` 모델 [[1]](https://huggingface.co/openai/clip-vit-base-patch32)과 `clip-benchmark/wds_imagenetv2` 데이터셋 [[2]](https://huggingface.co/datasets/clip-benchmark/wds_imagenetv2)을 사용한다. 각각을 사용하는 방법은 Hugging Face에서 Use this model(또는 dataset)을 참고한다.
* `openai/clip-vit-base-patch32`: CLIP 논문이 처음 공개되었을 당시 OpenAI에서 제공한 모델이다.
* `clip-benchmark/wds_imagenetv2`: 이미지(`webp`)와 클래스(`cls`)로 구성된 데이터 셋이다. 정수로 나타내어지는 클래스가 무엇을 의미하는지는 `classnames.txt` 파일을 통해 확인할 수 있다. 해당 데이터셋은 모델의 일반화(generalization) 성능을 확인하기 위한 것으로 `test` 데이터셋만 존재하며 기존의 ImageNet 데이터셋의 레이블과 일치하지만 새로운 이미지들로 구성되어 있다.

<br>

![dataset viewer](/assets/img/2025-07-17/dataset_viewer.png)

<br>

![wds_imagenetv2](/assets/img/2025-07-17/wds_imagenetv2.png)

<br>

## Zero-Shot Image Classification

<br>

CLIP 논문 [[3]](https://arxiv.org/abs/2103.00020)에서는 아래의 그림과 같이 이미지와 텍스트 사이의 연관성을 cosine similarity를 통해 나타낸다. 이미지 임베딩과 텍스트 임베딩을 동일한 임베딩 공간(embedding space)에 놓고 올바른 쌍의 cosine similarity는 최대화하고 잘못된 쌍의 cosine similarity는 최소화하는 식으로 이미지 인코더와 텍스트 인코더를 학습시킨다.

<br>

![cosine similarity from paper](/assets/img/2025-07-17/cosine_similarity_from_paper.png)

<br>

모델과 데이터셋을 불러오기에 앞서 필요한 라이브러리를 준비한다.

<br>

```python
# pip install transformers
# pip install datasets

from transformers import CLIPModel, CLIPProcessor
from datasets import load_dataset
```

<br>

`CLIPModel`은 우리가 사용하고자 하는 실제 CLIP 모델이다. `CLIPProcessor`는 데이터셋의 이미지와 텍스트를 CLIP 모델의 입력 형태로 가공한다.

<br>

아래의 코드를 실행해 모델과 데이터셋을 불러온다.

<br>

```python
# Load Model
model = CLIPModel.from_pretrained("openai/clip-vit-base-patch32")
processor = CLIPProcessor.from_pretrained("openai/clip-vit-base-patch32")

# Load Dataset
dataset = load_dataset("clip-benchmark/wds_imagenetv2")
test_dataset = dataset["test"]
```

<br>

이미지 데이터와 텍스트 데이터를 각각 이미지 임베딩과 텍스트 임베딩으로 변환한다. 이때 `processor`로 PIL 형식의 이미지를 텐서 형태로 변환하고 텍스트를 벡터 형태로 변환하는 (토크나이저) 과정을 거친다.

<br>

```python
sample_size = 10
subset = test_dataset.shuffle().select(range(sample_size))
images = list(subset["webp"])

cls2label = open("classnames.txt", "r").readlines()
label_texts = [cls2label[sub].rstrip() for sub in subset["cls"]]

inputs_image = processor(images=images, return_tensors="pt", padding=True)
inputs_text = processor(text=label_texts, return_tensors="pt", padding=True)

with torch.no_grad():
    image_embeddings = model.get_image_features(pixel_values=inputs_image["pixel_values"])
    text_embeddings = model.get_text_features(input_ids=inputs_text["input_ids"], attention_mask=inputs_text["attention_mask"])
```

<br>

```
# images
[<PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=1344x894 at 0x33F20AD20>, <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=468x312 at 0x33F4B9D60>, <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=500x375 at 0x33F4BAB10>, <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=500x333 at 0x33F4BBBC0>, <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=500x333 at 0x33F4B8F50>, <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=500x333 at 0x33F20A8D0>, <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=500x375 at 0x33F4B91C0>, <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=500x333 at 0x33F20A7B0>, <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=500x333 at 0x33F20A6F0>, <PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=500x500 at 0x33F20A630>]

# label_texts
['catamaran', 'basketball', 'manhole cover', 'Asian elephant', 'fire truck', 'paddle', 'smooth newt', 'red panda', 'Dutch oven', 'jeans']

# inputs_image
{'pixel_values': tensor([[[[ 1.1274e+00,  1.0982e+00,  1.1128e+00,  ...,  9.9604e-01,
            9.9604e-01,  9.9604e-01],
          [ 1.2296e+00,  1.2296e+00,  1.2442e+00,  ...,  1.1274e+00,
            1.1128e+00,  1.1274e+00],
          [ 1.2734e+00,  1.2442e+00,  1.2442e+00,  ...,  1.1128e+00,
            1.1128e+00,  1.1128e+00],
          ...,
          [ 3.2541e-02, -2.4483e-01, -2.5853e-02,  ...,  3.0991e-01,
            1.2013e-01,  1.6393e-01],
          [ 3.9750e-01,  3.8290e-01,  3.8290e-01,  ...,  7.4786e-01,
            7.4786e-01,  5.5808e-01],
          [ 3.5371e-01,  9.0935e-02, -6.9648e-02,  ...,  3.3911e-01,
            2.8071e-01,  4.8509e-01]],

         [[ 1.5346e+00,  1.5346e+00,  1.5346e+00,  ...,  1.1594e+00,
            1.1594e+00,  1.1594e+00],
          [ 1.6697e+00,  1.6847e+00,  1.6847e+00,  ...,  1.2945e+00,
            1.2795e+00,  1.2945e+00],
          [ 1.6697e+00,  1.6697e+00,  1.6547e+00,  ...,  1.2795e+00,
            1.2795e+00,  1.2795e+00],
          ...,
          [ 2.5894e-01,  3.8118e-03,  2.4394e-01,  ...,  5.5910e-01,
            3.3398e-01,  3.7901e-01],
          [ 6.0412e-01,  6.4915e-01,  6.4915e-01,  ...,  9.4930e-01,
            9.7932e-01,  7.8422e-01],
          [ 5.2908e-01,  3.1897e-01,  1.8391e-01,  ...,  5.1408e-01,
            4.6905e-01,  6.9417e-01]],

         [[ 1.9468e+00,  1.9326e+00,  1.9042e+00,  ...,  1.3496e+00,
            1.3496e+00,  1.3496e+00],
          [ 2.1459e+00,  2.1317e+00,  2.0890e+00,  ...,  1.4776e+00,
            1.4633e+00,  1.4776e+00],
          [ 2.0890e+00,  2.1317e+00,  2.0890e+00,  ...,  1.4633e+00,
            1.4633e+00,  1.4633e+00],
          ...,
          [ 6.3857e-01,  4.1105e-01,  6.1013e-01,  ...,  8.6609e-01,
            6.6701e-01,  7.0967e-01],
          [ 7.3811e-01,  7.6655e-01,  7.5233e-01,  ...,  1.1932e+00,
            1.2358e+00,  1.0652e+00],
          [ 7.8077e-01,  5.6747e-01,  4.2527e-01,  ...,  7.5233e-01,
            6.9545e-01,  9.0875e-01]]],


        ...,


        [[[-1.3397e+00, -1.1937e+00, -1.1791e+00,  ...,  2.5152e-01,
            3.8290e-01,  6.3108e-01],
          [-1.3835e+00, -1.2667e+00, -1.1937e+00,  ...,  2.2232e-01,
            3.5371e-01,  6.4567e-01],
          [-1.3689e+00, -1.2667e+00, -1.0769e+00,  ...,  2.6612e-01,
            3.2451e-01,  6.1648e-01],
          ...,
          [-1.2083e+00, -1.1061e+00, -1.0915e+00,  ..., -6.0979e-01,
           -6.2439e-01, -6.3899e-01],
          [-1.2083e+00, -1.0769e+00, -1.0915e+00,  ..., -5.0760e-01,
           -5.0760e-01, -5.2220e-01],
          [-1.2083e+00, -1.0915e+00, -1.0623e+00,  ..., -4.0541e-01,
           -4.2001e-01, -4.4921e-01]],

         [[-1.1968e+00, -1.0317e+00, -9.5669e-01,  ...,  3.6400e-01,
            5.4409e-01,  8.2924e-01],
          [-1.2418e+00, -1.1068e+00, -9.7169e-01,  ...,  3.7901e-01,
            5.1408e-01,  8.2924e-01],
          [-1.2118e+00, -1.1068e+00, -8.6664e-01,  ...,  4.5404e-01,
            4.8406e-01,  7.8422e-01],
          ...,
          [-1.0918e+00, -9.8670e-01, -9.7169e-01,  ..., -3.1135e-01,
           -3.5637e-01, -3.5637e-01],
          [-1.0918e+00, -9.5669e-01, -9.7169e-01,  ..., -2.2130e-01,
           -2.3631e-01, -2.5132e-01],
          [-1.0918e+00, -9.7169e-01, -9.4168e-01,  ..., -1.3126e-01,
           -1.3126e-01, -1.4627e-01]],

         [[-7.4078e-01, -5.4170e-01, -4.7060e-01,  ...,  1.1505e+00,
            1.2358e+00,  1.3922e+00],
          [-7.9766e-01, -6.1280e-01, -4.8482e-01,  ...,  1.1221e+00,
            1.2074e+00,  1.4207e+00],
          [-7.5500e-01, -6.2702e-01, -3.8527e-01,  ...,  1.1647e+00,
            1.1789e+00,  1.3922e+00],
          ...,
          [-6.9812e-01, -5.9858e-01, -5.8436e-01,  ...,  4.9637e-01,
            4.5371e-01,  4.1105e-01],
          [-6.9812e-01, -5.5592e-01, -5.8436e-01,  ...,  5.8169e-01,
            5.6747e-01,  5.3903e-01],
          [-7.1234e-01, -5.5592e-01, -5.5592e-01,  ...,  6.6701e-01,
            6.5279e-01,  6.2435e-01]]]])}

# inputs_image.["pixel_values"].shape
torch.Size([10, 3, 224, 224])

# inputs_text
{'input_ids': tensor([[49406,  1481,  6424,   550, 49407],
        [49406,  3835, 49407, 49407, 49407],
        [49406,   723,  5341,  2202, 49407],
        [49406,  7128, 10299, 49407, 49407],
        [49406,  1769,  4629, 49407, 49407],
        [49406, 20811, 49407, 49407, 49407],
        [49406,  8990, 37224, 49407, 49407],
        [49406,   736, 12952, 49407, 49407],
        [49406,  7991, 12579, 49407, 49407],
        [49406, 10157, 49407, 49407, 49407]]), 'attention_mask': tensor([[1, 1, 1, 1, 1],
        [1, 1, 1, 0, 0],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 0],
        [1, 1, 1, 1, 0],
        [1, 1, 1, 0, 0],
        [1, 1, 1, 1, 0],
        [1, 1, 1, 1, 0],
        [1, 1, 1, 1, 0],
        [1, 1, 1, 0, 0]])}

# inputs_text["input_ids"]
tensor([[49406,  1481,  6424,   550, 49407],
        [49406,  3835, 49407, 49407, 49407],
        [49406,   723,  5341,  2202, 49407],
        [49406,  7128, 10299, 49407, 49407],
        [49406,  1769,  4629, 49407, 49407],
        [49406, 20811, 49407, 49407, 49407],
        [49406,  8990, 37224, 49407, 49407],
        [49406,   736, 12952, 49407, 49407],
        [49406,  7991, 12579, 49407, 49407],
        [49406, 10157, 49407, 49407, 49407]])

# inputs_text["attention_mask"]
tensor([[1, 1, 1, 1, 1],
        [1, 1, 1, 0, 0],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 0],
        [1, 1, 1, 1, 0],
        [1, 1, 1, 0, 0],
        [1, 1, 1, 1, 0],
        [1, 1, 1, 1, 0],
        [1, 1, 1, 1, 0],
        [1, 1, 1, 0, 0]])

# image_embeddings
tensor([[ 0.0046, -0.2677,  0.0479,  ..., -0.0996,  0.3882, -0.3725],
        [ 0.0161, -0.0086,  0.0370,  ...,  0.0970, -0.4052, -0.1407],
        [-0.4285,  0.0897,  0.0933,  ...,  0.5300, -0.3923,  0.1011],
        ...,
        [-0.0852, -0.0968,  0.3105,  ...,  0.2392,  0.0327, -0.1320],
        [-0.3767,  0.5803, -0.2108,  ...,  0.7641,  0.0388, -0.3238],
        [-0.2243,  0.0426,  0.3244,  ...,  0.9456, -0.1038,  0.4206]])

# image_embeddings.shape
torch.Size([10, 512])

# text_embeddings
tensor([[ 0.2074, -0.1108,  0.2210,  ...,  0.0251, -0.0259, -0.1902],
        [-0.0447,  0.0232, -0.2639,  ...,  0.5787, -0.5239, -0.3520],
        [-0.3476, -0.1130,  0.2891,  ...,  0.2645, -0.0355,  0.0041],
        ...,
        [ 0.3439,  0.2105,  0.3439,  ...,  0.3410, -0.5239, -0.1157],
        [-0.1396, -0.0562, -0.2503,  ...,  0.0902, -0.6934, -0.0777],
        [ 0.3459,  0.1091, -0.2213,  ..., -0.0261,  0.0547,  0.4197]])

# text_embeddings.shape
torch.Size([10, 512])
```

<br>

* `inputs_image`: 10 장의 3 채널의 224×224 크기의 이미지
* `inputs_text["input_ids"]`: [SOS] 토큰: 49406 / [EOS] 토큰 (또는 공백): 49407 
* `inputs_text["attention_mask"]`: 0에 해당하는 토큰은 연산하지 않는다. 문장과 상관없는 토큰은 attention_mask를 꺼서 연산에 포함시키지 않는다.
* `image_embeddings`과 `text_embeddings`: 동일한 크기의 벡터로 변환된 것을 확인할 수 있다. 임베딩 벡터의 크기는 사용하는 모델에 따라 다르다.

<br>

예를 들어, 토큰화를 거친 "red panda"는 [[SOS], "red", "panda", [EOS], [EOS]] = [49406, 736, 12952, 49407, 49407]로 대응된다.

<br>

이미지 임베딩과 텍스트 임베딩을 각각 크기가 1인 벡터로 변환한 다음 둘 사이의 cosine similarity를 계산한다.

<br>

```python
image_embeddings = image_embeddings / image_embeddings.norm(dim=-1, keepdim=True)
text_embeddings = text_embeddings / text_embeddings.norm(dim=-1, keepdim=True)

similarity_matrix = cosine_similarity(image_embeddings.cpu().numpy(), text_embeddings.cpu().numpy())
```

<br>

```
# image_embeddings
tensor([[ 0.0005, -0.0270,  0.0048,  ..., -0.0100,  0.0391, -0.0375],
        [ 0.0017, -0.0009,  0.0038,  ...,  0.0100, -0.0418, -0.0145],
        [-0.0417,  0.0087,  0.0091,  ...,  0.0515, -0.0382,  0.0098],
        ...,
        [-0.0082, -0.0093,  0.0297,  ...,  0.0229,  0.0031, -0.0126],
        [-0.0369,  0.0568, -0.0206,  ...,  0.0748,  0.0038, -0.0317],
        [-0.0204,  0.0039,  0.0296,  ...,  0.0862, -0.0095,  0.0383]])

# text_embeddings
tensor([[ 0.0223, -0.0119,  0.0238,  ...,  0.0027, -0.0028, -0.0205],
        [-0.0042,  0.0022, -0.0247,  ...,  0.0541, -0.0490, -0.0329],
        [-0.0380, -0.0123,  0.0316,  ...,  0.0289, -0.0039,  0.0005],
        ...,
        [ 0.0449,  0.0275,  0.0449,  ...,  0.0445, -0.0684, -0.0151],
        [-0.0146, -0.0059, -0.0261,  ...,  0.0094, -0.0724, -0.0081],
        [ 0.0364,  0.0115, -0.0233,  ..., -0.0027,  0.0058,  0.0442]])

# similarity_matrix
array([[0.24136572, 0.1250148 , 0.11022732, 0.10075002, 0.14235449,
        0.21465015, 0.12760073, 0.09021723, 0.18018813, 0.14756934],
       [0.17426471, 0.2802984 , 0.17706944, 0.17627603, 0.19559409,
        0.2128358 , 0.15687756, 0.17561541, 0.2250892 , 0.17089693],
       [0.1793225 , 0.19192825, 0.3196076 , 0.19511038, 0.17691006,
        0.21382916, 0.20893317, 0.19213325, 0.23087528, 0.15962636],
       [0.14638281, 0.19194035, 0.15271313, 0.26126093, 0.16300671,
        0.18634588, 0.11470719, 0.15328509, 0.17345087, 0.16508152],
       [0.1723489 , 0.15916066, 0.16656843, 0.12937905, 0.26392016,
        0.16438459, 0.11530264, 0.11246548, 0.18222436, 0.12582672],
       [0.24887867, 0.16010484, 0.13833493, 0.19219376, 0.1674177 ,
        0.26140624, 0.18791659, 0.13797131, 0.2175976 , 0.15310816],
       [0.19629262, 0.18293281, 0.21275909, 0.14318511, 0.14682595,
        0.18439701, 0.30583256, 0.15720026, 0.20187661, 0.12669842],
       [0.16171508, 0.18179381, 0.13085176, 0.19845869, 0.18184525,
        0.2020441 , 0.14198066, 0.3111505 , 0.17165472, 0.14233361],
       [0.15284483, 0.15180184, 0.14745386, 0.20017263, 0.14195284,
        0.17671347, 0.14749712, 0.12193655, 0.2296159 , 0.10747041],
       [0.16689423, 0.20894916, 0.18819004, 0.19782262, 0.18993646,
        0.2061661 , 0.18267117, 0.14346378, 0.21319968, 0.25287104]],
      dtype=float32)
```

<br>

아래의 그림은 계산한 cosine similarity를 히트맵(heatmap)으로 나타낸 것이다.

<br>

![cosine similarity](/assets/img/2025-07-17/cosine_similarity.png)

<br>

Cosine similarity가 클수록 두 벡터는 유사하며 히트맵 상에서 빨간색으로 표현되었다. 즉, 대각선에 놓인 이미지와 텍스트는 서로 연관성이 높다고 할 수 있다.

<br>

---

## References
 
[1] <https://huggingface.co/openai/clip-vit-base-patch32>  
[2] <https://huggingface.co/datasets/clip-benchmark/wds_imagenetv2>  
[3] <https://arxiv.org/abs/2103.00020>