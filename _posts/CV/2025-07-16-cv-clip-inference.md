---
title: "CLIP Model Inference"
date: 2025-07-16 10:35:00 +0900
categories: CV
---

&nbsp;

Hugging Face(🤗)에 있는 모델과 데이터셋을 사용한다.

<br>

![wds_imagenetv2](assets\img\2025-07-16-cv-clip-inference\2025-07-16-clip-inference-dataset.png)

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

`from_pretrained` 함수를 통해 Hugging Face에 있는 모델을 불러온다. 데이터셋은 모델의 인퍼런스를 위한 `test` 파일을 가져온다. (모델과 데이터셋의 사용법은 Hugging Face의 해당 페이지에서 Use this model을 통해 확인할 수 있다.)

<br>

---

## References

[1] <https://huggingface.co/openai/clip-vit-base-patch32>  
[2] <https://huggingface.co/datasets/clip-benchmark/wds_imagenetv2>  

&nbsp;