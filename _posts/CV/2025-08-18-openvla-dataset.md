---
title: "[OpenVLA] OpenVLA 모델 추론 1) Open X-Embodiment Dataset"
date: 2025-08-18 10:50:00 +0900
categories: [CV, Multi Modal]
---

&nbsp;

이번 포스팅에서는 OpenVLA 모델의 훈련 데이터셋인 Open X-Embodiment 데이터셋에 대해 살펴본다.

<br>

## Open X-Embodiment Dataset

<br>

Open X-Embodiment (OpenX) 데이터셋은 97만 개의 로봇 에피소드로 구성된 대규모 로봇 훈련 데이터셋 [[1]](<https://arxiv.org/abs/2406.09246>)이다. 로봇의 동작은 순차적인(sequential) 특성을 갖는다. 상황(프레임)이 바뀔 때마다 로봇이 수행해야 하는 행동도 바뀌게 된다. 그렇기 때문에 로봇의 훈련 데이터셋은 지시문(instruction)과 사람이 수행한 영상을 프레임 단위로 나눈 이미지(image) 데이터로 구성된다.

<br>

![970k robot episodes](/assets/img/2025-08-18/970k-robot-episodes.png)

<br>

## Load from Hugging Face

<br>

데이터셋을 직접 로드해 어떤 식으로 구성되어 있는지 확인한다. OpenX 데이터셋은 공개 데이터셋으로, Colab 링크 [[2]](<https://colab.research.google.com/github/google-deepmind/open_x_embodiment/blob/main/colabs/Open_X_Embodiment_Datasets.ipynb>)를 통해 데이터셋을 다운로드해서 사용하거나 Hugging Face(🤗)로부터 데이터셋을 로드해서 사용할 수 있다. 다음 포스팅에서 OpenVLA 모델을 Huggle Face에서 불러올 것이기 때문에 구현하기 편하도록 후자의 방법을 택한다.

<br>

![Hugging Face](/assets/img/2025-08-18/Hugging-Face.png)

<br>

아래의 그림은 OpenX 데이터셋의 서브셋(subset) 목록이다. 97만 개의 에피소드를 구성하는 서브 에피소드들이라고 할 수 있다. 이름에서 알 수 있듯 다양한 종류의 태스크들로 구성되어 있다. 태스크에 따라 로봇과 배경, 행동 등이 바뀌게 되는데 OpenVLA 모델이 이러한 다양성을 학습했기 때문에 기존 VLA 모델에 비해서 일반화 성능이 크게 향상되었다.

<br>

![Optional subdatasets](/assets/img/2025-08-18/optional-subdatasets.png)

<br>

Hugging Face의 `datasets` 라이브러리로 수많은 데이터 중 일부를 서브셋으로 지정해 특정 태스크의 데이터를 로드할 수 있다. 아래 코드는 Hugging Face의 사용 예시이다.

<br>

```python
# Hugging Face의 Usage Example

import datasets
ds = datasets.load_dataset("jxu124/OpenX-Embodiment", "fractal20220817_data", streaming=True, split='train')
```

<br>

이번 포스팅에서는 비교적 작은 용량인 `nyu_door_opening_surprising_effectiveness` 서브셋을 다룬다. (서브셋별로 용량은 천차만별이고 100GB를 넘기는 것도 있다.)

<br>

```python
import datasets
import io

from PIL import Image
from matplotlib import pyplot as plt

ds = datasets.load_dataset("jxu124/OpenX-Embodiment", "nyu_door_opening_surprising_effectiveness", trust_remote_code=True)

samples = ds['train'][0]['data.pickle']['steps']

images = []
for sample in samples:
    encoded_image = sample['observation']['image']['bytes']
    instruct = sample['observation']['natural_language_instruction']

    image = Image.open(io.BytesIO(encoded_image))
    images.append(image)
```

<br>

```
# print(ds)
DatasetDict({
    train: Dataset({
        features: ['__key__', '__url__', 'data.pickle', '__local_path__'],
        num_rows: 435
    })
})

# print(instruction)
b'open door'

# ds['train'][0]
{'__key__': 'sample_000000000000', '__url__': '/Users/osuyeon/.cache/huggingface/hub/datasets--jxu124--OpenX-Embodiment/snapshots/3c...opening_surprising_effectiveness_00000.tar', 'data.pickle': {'image_list': [...], 'steps': [...]}, '__local_path__': '/Users/osuyeon/.cache/huggingface/hub/datasets--jxu124--OpenX-Embodiment/snapshots/3c...opening_surprising_effectiveness_00000.tar'}

# ds['train'][0].keys()
dict_keys(['__key__', '__url__', 'data.pickle', '__local_path__'])

# ds['train'][0]['data.pickle']
{'image_list': ['image'], 'steps': [{...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, ...]}

# ds['train'][0]['data.pickle'].keys()
dict_keys(['image_list', 'steps'])

# ds['train'][0]['data.pickle']['image_list']
['image']

# ds['train'][0]['data.pickle']['image_list']['image']
Traceback (most recent call last):
  File "<string>", line 1, in <module>
TypeError: list indices must be integers or slices, not str

# ds['train'][0]['data.pickle']['steps']
[{'action': {...}, 'is_first': True, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': False, 'is_terminal': False, 'observation': {...}, 'reward': 0.0}, {'action': {...}, 'is_first': False, 'is_last': True, 'is_terminal': True, 'observation': {...}, 'reward': 1.0}, {'action': {...}, 'is_first': False, 'is_last': True, 'is_terminal': True, 'observation': {...}, 'reward': 0.0}]

# len(samples) (프레임 개수)
46

# sample
{'action': {'gripper_closedness_action': [...], 'rotation_delta': [...], 'terminate_episode': 0.0, 'world_vector': [...]}, 'is_first': True, 'is_last': False, 'is_terminal': False, 'observation': {'image': {...}, 'natural_language_embedding': [...], 'natural_language_instruction': b'open door'}, 'reward': 0.0}

# sample.keys() (is_: 첫 번째 프레임, 마지막 프레임, 종료되었는가)
dict_keys(['action', 'is_first', 'is_last', 'is_terminal', 'observation', 'reward'])

# sample['action'] (사람이 수행한 로봇의 움직임 정보 = 정답)
{'gripper_closedness_action': [-0.00024771690368652344], 'rotation_delta': [-0.004529649391770363, 0.005992727819830179, 0.004243004601448774], 'terminate_episode': 0.0, 'world_vector': [-9.537393634673208e-05, 0.0012610105331987143, -0.003427658462896943]}

# sample['action'].keys()
dict_keys(['gripper_closedness_action', 'rotation_delta', 'terminate_episode', 'world_vector'])

# sample['observation'] (image와 insturction)
{'image': {'bytes': b'\xff\xd8\xff\xe0\x00\x10JFIF\x00\x01\x01\x00\x00\x01\x00\x01\x00\x00\xff\xdb\x00C\x00\x08\x06\x06\x07\x06\x05\x08\x07\x07\x07\t\t\x08\n\x0c\x14\r\x0c\x0b\x0b\x0c\x19\x12\x13\x0f\x14\x1d\x1a\x1f\x1e\x1d\x1a\x1c\x1c $.\' ",#\x1c\x1c(7),01444\x1f\'9=82<...\xbe\xf3\xd4{w=Ic\x97\xe6\x7f\x96\x8f\xf6\x9a\x9c\xb8\xf9\xbf\xef\x9aj\xfd\xca\x00j\xe3\xe6j)\xdb~E\xa7|\xbf\xf8\xee\xda\x00\xff\xd9', 'path': None}, 'natural_language_embedding': [0.004324073437601328, -0.012277779169380665, -0.03588211536407471, 0.06965800374746323, -0.02430354617536068, -0.022569917142391205, -0.006370545830577612, -0.044684361666440964, 0.05525897070765495, -0.042060744017362595, 0.027653323486447334, -0.00270639406517148, 0.009368452243506908, -0.07284097373485565, -0.0626731812953949, 0.048572614789009094, 0.06526853889226913, -0.06726699322462082, -0.048759575933218, ...], 'natural_language_instruction': b'open door'}

# sample['observation'].keys()
dict_keys(['image', 'natural_language_embedding', 'natural_language_instruction'])
```

<br>

## Convert Subdataset to GIF

<br>

서브셋을 하나의 GIF 파일로 변환해 어떤 태스크인지 확인한다. `nyu_door_opening_surprising_effectiveness` 서브셋은 총 46개의 프레임으로 구성된 것을 확인할 수 있다.

<br>

![nyu_door_opening_surprising_effectiveness (PNG)](/assets/img/2025-08-18/nyu_door_opening_surprising_effectiveness.png)

<br>

로봇 팔이 서랍의 문을 여는 태스크로, 서랍의 손잡이를 잡을 수 있도록 로봇 팔의 위치와 각도를 조정하고 문을 집어서 당기는 작업을 수행한다.

<br>

![nyu_door_opening_surprising_effectiveness (GIF)](/assets/img/2025-08-18/nyu_door_opening_surprising_effectiveness.gif)

<br>

이러한 이미지와 지시문을 VLA 모델의 입력으로 주면 로봇의 행동을 모델의 출력으로 얻을 수 있다. 다음 포스팅에서는 OpenVLA 모델을 로봇 태스크에 직접 적용해 보도록 한다.

<br>

## Another Subdatasets

<br>

* **berkeley_cable_routing** (instruction: b'route cable')

![berkeley_cable_routing (PNG)](/assets/img/2025-08-18/berkeley_cable_routing.png)|![berkeley_cable_routing (GIF)](/assets/img/2025-08-18/berkeley_cable_routing.gif)

<br>

* **toto** (instruction: b'pour')(Time Elapsed: 26:03)

![toto (PNG)](/assets/img/2025-08-18/toto.png)|![toto (GIF)](/assets/img/2025-08-18/toto.gif)

<br>

## Error

<br>

<div class="box-danger">
<div class="title"> (Code 7) RuntimeError: Dataset scripts are no longer supported, but found OpenX-Embodiment. py </div>
Hugging Face의 datasets 라이브러리가 업데이트되면서 더 이상 로컬 Python 스크립트(.py) 기반의 데이터셋 로딩 방식을 지원하지 않는다는 에러이다. 이를 해결하기 위해 라이브러리의 버전을 (datasets==4.0.0에서 3.6.0으로) 다운그레이드 했다.
<br>
<br>
>> <code>pip install "datasets<4.0.0"</code>
</div>

<br>

---

## References
[1] <https://arxiv.org/abs/2406.09246>  
[2] <https://colab.research.google.com/github/google-deepmind/open_x_embodiment/blob/main/colabs/Open_X_Embodiment_Datasets.ipynb>