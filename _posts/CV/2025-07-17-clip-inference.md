---
title: "CLIP Model Inference - Cosine similarity"
date: 2025-07-16 10:35:00 +0900
categories: CV
---

&nbsp;

![cosine similarity from paper](assets\img\2025-07-16-cv-clip-inference\cosine_similarity_from_paper.png)

<br>

논문을 보면 이미지와 텍스트 사이의 연관성을 cosine similarity를 통해 보여준다.

<br>

Hugging Face(🤗)에 있는 모델과 데이터셋을 사용한다.

<br>

데이터셋 같은 경우 모델의 일반화 성능을 테스트 하기 위한 데이터셋이므로 Test 데이터 셋만 존재한다.


모델과 데이터셋의 사용법은 Hugging Face의 해당 페이지에서 Use this model(or dataset)을 통해 확인할 수 있다.

<br>

![wds_imagenetv2](assets\img\2025-07-16-cv-clip-inference\wds_imagentv2.png)

<br>

모델과 데이터셋을 불러오기에 앞서 필요한 라이브러리를 import한다.

<br>

```python
# pip install transformers
# pip install datasets

from transformers import CLIPModel, CLIPProcessor
from datasets import load_dataset
```


<br>

`CLIPModel`은 우리가 사용하고자 하는 CLIP 모델이다. `CLIPProcessor`는 데이터셋의 이미지와 텍스트를 CLIP 모델의 입력 형태로 가공한다. 가공된 데이터를 CLIP 모델에 입력하면 임베딩 스페이스 상에서 어떤 벡터 형태로 나타나는지 알 수 있다.

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

`from_pretrained` 함수를 통해 Hugging Face에 있는 모델을 불러온다. 모델의 인퍼런스를 위한 `test` 데이터셋을 가져온다. 모델과 데이터셋 이름은 Huggine Face 페이지에서 복사/붙여넣는다.

<br>

다음은 이미지 데이터와 텍스트 데이터를 각각 임베딩 벡터로 만든다.

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

이미지 임베딩과 텍스트 임베딩을 각각 크기가 1인 벡터로 변환한 다음 cosine similarity를 계산한다.

<br>

```python
image_embeddings = image_embeddings / image_embeddings.norm(dim=-1, keepdim=True)
text_embeddings = text_embeddings / text_embeddings.norm(dim=-1, keepdim=True)

similarity_matrix = cosine_similarity(image_embeddings.cpu().numpy(), text_embeddings.cpu().numpy())
```

<br>

![cosine similarity](assets\img\2025-07-16-cv-clip-inference\cosine_similarity.png)

<br>

앞서 랜덤하게 선택한 10장의 이미지와 텍스트 사이의 연관성, cosine similarity를 히트맵으로 나타냈다. 연관성이 높을수록 빨갛게 표현된다.대각선 부분이 정답이라고 할 수 있다. 이미지와 텍스트의 similarity가 높은 것들이 대각선에 위치한다.

<br>

이러한 방법을 통해 CLIP 모델이 classification을 잘 수행했는지 판단할 수 있다.


<br>

---

## References

[1] <https://arxiv.org/abs/2103.00020>  
[2] <https://huggingface.co/openai/clip-vit-base-patch32>  
[3] <https://huggingface.co/datasets/clip-benchmark/wds_imagenetv2>  

&nbsp;