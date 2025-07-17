---
title: "CLIP Model Inference"
date: 2025-07-17 09:57:00 +0900
categories: CV
---

&nbsp;

이번 포스팅에서는 CLIP 모델을 직접 사용해 보고 그 결과를 시각화한다.

<br>

## Model & Dataset

<br>

모델과 데이터셋은 Hugging Face(🤗)의 `openai/clip-vit-base-patch32` 모델[[1]](https://huggingface.co/openai/clip-vit-base-patch32)과 `clip-benchmark/wds_imagenetv2` 데이터셋[[2]](https://huggingface.co/datasets/clip-benchmark/wds_imagenetv2)을 사용한다. 각각의 사용법은 Hugging Face의 해당 페이지에서 Use this model/dataset을 통해 확인할 수 있다.
* `openai/clip-vit-base-patch32`: CLIP 논문이 처음 공개되었을 당시 OpenAI에서 제공한 모델이다. 베이스 모델이고 패치 사이즈는 32이다.
* `clip-benchmark/wds_imagenetv2`: 이미지(`webp`)와 클래스(`cls`)로 구성된 데이터 셋이다. 정수로 나타내어지는 클래스가 무엇을 의미하는지는 `classnames.txt` 파일을 통해 확인할 수 있다. 이 데이터셋은 모델의 일반화(generalization) 성능을 확인하기 위한 것으로 `test` 데이터셋만 존재한다.

<br>

![dataset viewer](/assets/img/2025-07-17/dataset_viewer.png)

<br>

![wds_imagenetv2](/assets/img/2025-07-17/wds_imagenetv2.png)

<br>

## Zero-Shot Classification

<br>

CLIP 논문[[3]](https://arxiv.org/abs/2103.00020)에서는 아래의 그림과 같이 이미지와 텍스트 사이의 연관성을 cosine similarity를 통해 나타낸다. 이미지 임베딩과 텍스트 임베딩을 동일한 임베딩 공간(embedding space)에 놓고 올바른 쌍의 cosine similarity는 최대화하고 잘못된 쌍의 cosine similarity는 최소화하는 식으로 이미지 인코더와 텍스트 인코더를 학습시킨다.

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

`CLIPModel`은 우리가 사용하고자 하는 CLIP 모델이다. `CLIPProcessor`는 데이터셋의 이미지와 텍스트를 CLIP 모델의 입력 형태로 가공한다.

<br>

아래의 코드를 통해 모델과 데이터셋을 불러온다.

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

`from_pretrained()` 함수를 통해 Hugging Face에 있는 모델을 불러온다. 모델의 인퍼런스(inference)를 위한 `test` 데이터셋을 가져온다.

<br>

다음은 이미지 데이터와 텍스트 데이터를 각각 이미지 임베딩과 텍스트 임베딩으로 만든다.

<br>

```python
sample_size = 10
subset = test_dataset.shuffle().select(range(sample_size))
images = list(subset["webp"])

cls2label = open("classnames.txt", "r").readlines()
label_texts = [cls2label[sub].rstrip() for sub in subset["cls"]]

with torch.no_grad():
    image_embeddings = model.get_image_features(pixel_values=inputs_image["pixel_values"])
    text_embeddings = model.get_text_features(input_ids=inputs_text["input_ids"], attention_mask=inputs_text["attention_mask"])
```

<br>

이미지 임베딩과 텍스트 임베딩을 각각 크기가 1인 벡터로 변환한 다음 둘 사이의 cosine similarity를 계산한다.

<br>

```python
image_embeddings = image_embeddings / image_embeddings.norm(dim=-1, keepdim=True)
text_embeddings = text_embeddings / text_embeddings.norm(dim=-1, keepdim=True)

similarity_matrix = cosine_similarity(image_embeddings.cpu().numpy(), text_embeddings.cpu().numpy())
```

<br>

아래의 그림은 앞서 무작위로 선택한 10장의 이미지와 텍스트 사이의 cosine similarity를 히트맵(heatmap)으로 나타낸 것이다.

<br>

![cosine similarity](/assets/img/2025-07-17/cosine_similarity.png)

<br>

Cosine similarity가 클수록 두 벡터는 유사하며 히트맵 상에서 빨간색으로 표현된다. 즉, 대각선에 놓인 이미지와 텍스트는 연관성이 높다고 할 수 있다.

<br>

## Embedding Space Visualization

<br>

See you soon...

<br>

---

## References
 
[1] <https://huggingface.co/openai/clip-vit-base-patch32>  
[2] <https://huggingface.co/datasets/clip-benchmark/wds_imagenetv2>  
[3] <https://arxiv.org/abs/2103.00020>

&nbsp;