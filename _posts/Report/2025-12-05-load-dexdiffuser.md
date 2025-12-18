---
title: "[12월 2주차] 파지 모델 불러오기 (with DexDiffuser)"
date: 2025-12-05 13:38:00 +0900
categories: [Report]
---
---

## DexDiffusion GitHub

* <https://github.com/YuLiHN/DexDiffuser>
* `git clone https://github.com/YuLiHN/DexDiffuser.git`

<br>

> ## [25/12/08] DexDiffuser 환경 구축 [✗]
> * README.md에 명시된 대로 dexdiff 환경 구성 (Python: 3.8 / Torch: 2.0.1 / CUDA: 11.7)
> * dexdiff 환경에 설치된 PyTorch 버전과 시스템 GPU 버전이 호환되지 않아 CUDA 에러 발생

<br>

#### Installation

<br>

* Create a conda environment
    * `conda create -n dexdiff python=3.8`
    * `conda activate dexdiff`

* Install CUDA 11.7
    * `conda install cudatoolkit==11.7 -c nvidia`
    * `conda install -c nvidia cuda-compiler=11.7 cuda-nvcc=11.7`
    * (확인) `nvcc --version`
    * `pip install torch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 --index-url https://download.pytorch.org/whl/cu117`
    * (확인) `python -c "import torch; print(torch.cuda.is_available()); print(torch.version.cuda)"`

* Install pytorch3d
    * `pip install fvcore iopath`
    * `pip install --no-index --no-cache-dir pytorch3d -f https://dl.fbaipublicfiles.com/pytorch3d/packaging/wheels/py38_cu117_pyt201/download.html`

* Install bps_torch
    * `pip install git+https://github.com/otaheri/chamfer_distance`
    * `pip install git+https://github.com/otaheri/bps_torch`

* Install dependencies
    * `pip install omegaconf einops urdf-parser-py hydra-core loguru plotly tqdm transformations trimesh matplotlib pyrender tensorboard tqdm transforms3d`
    * (추가) `pip install pytorch_kinematics`
    * (추가) `pip install flask`

* Install Isaac Gym [[1]](<https://menggu1234.tistory.com/25>)
    * <https://developer.nvidia.com/isaac-gym>
    * `cd ~/isaacgym/python`
    * `pip install -e .`
    * (필수) `sudo cp /home/suyeonoh/anaconda3/envs/dexdiff/lib/libpython3.8.so.1.0 /usr/lib/`
    * (선택) `cd ~/isaacgym/python/examples`
    * (선택) `python joint_monkey.py`

<br>

```bash
# nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2022 NVIDIA Corporation
Built on Wed_Jun__8_16:49:14_PDT_2022
Cuda compilation tools, release 11.7, V11.7.99
Build cuda_11.7.r11.7/compiler.31442593_0

# python -c "import torch; print(torch.cuda.is_available()); print(torch.version.cuda)"
True
11.7
```

<br>

#### Checkpoints & Data

<br>

* Checkpoints for sampler and evaluator
    * `ckpts` 폴더 생성
    * `ckpts` 폴더 내 weights 배치
        * `sampler` (`model_200.pth`)
        * `evaluator` (`model_20.pth`)

* Training data
    * `data` 폴더 생성
    * `data` 폴더 내 `object.zip` 압축 해제
    * `dexdiffuser_data` 폴더 생성
    * `dexdiffuser_data` 폴더 내 `.pickle` 파일 배치
        * `pc_data_egad.pickle`
        * `pc_data_kit.pickle`
        * `pc_data_multidex.pickle`
        * `train_succ.pt`
        * `obj_bps_dist_full.pt`

* config 파일의 경로 수정
    * `~/DexDiffuser/configs/task/evaluator_dexgn_slurm.yaml` - `object_root`, `data_root`
    * `~/DexDiffuser/configs/task/grasp_gen_ur_dexgn_slurm.yaml` - `object_root`, `data_root`
    * `~/DexDiffuser/configs/sample.yaml` - `data_root`, `sampler_bps_ckpt_pth`, `sampler_pn2_ckpt_pth`

<br>

![Screenshot 1](/assets/img/2025-12-05/screenshot-1.png)|![Screenshot 2](/assets/img/2025-12-05/screenshot-2.png)|![Screenshot 3](/assets/img/2025-12-05/screenshot-3.png)

<br>

#### Error

<br>

![Error](/assets/img/2025-12-05/error.png)

<br>

> ## [25/12/10] DexDiffuser 환경 구축 [✓]
> * 시스템 GPU 버전에 맞춰 dexdiff2 환경 구성 (Python: 3.9 / Torch: 2.8.0 / CUDA: 12.8)
> * (학습 중) (20/200 Epoch)

<br>

#### Installation

<br>

* `conda create -n dexdiff2 python=3.9`

* `conda activate dexdiff2`

* `conda install nvidia::cuda==12.8.0`
(참고: https://anaconda.org/channels/nvidia/packages/cuda/overview)

* `pip install torch==2.8.0 torchvision==0.23.0 torchaudio==2.8.0 --index-url https://download.pytorch.org/whl/cu128`
(참고: https://github.com/facebookresearch/pytorch3d/issues/1962)

* `pip install --extra-index-url https://miropsota.github.io/torch_packages_builder pytorch3d==0.7.9+pt2.8.0cu128`

* `pip install git+https://github.com/otaheri/chamfer_distance`

* `pip install git+https://github.com/otaheri/bps_torch`

* `pip install omegaconf einops urdf-parser-py hydra-core loguru plotly tqdm transformations trimesh matplotlib pyrender tensorboard tqdm transforms3d`

* `pip install pytorch_kinematics`

* `pip install flask`

<br>

```bash
# python --version
Python 3.9.25

# nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2025 NVIDIA Corporation
Built on Wed_Jan_15_19:20:09_PST_2025
Cuda compilation tools, release 12.8, V12.8.61
Build cuda_12.8.r12.8/compiler.35404655_0

# python -c "import torch; print(torch.cuda.is_available()); print(torch.version.cuda)"
True
12.8
```

<br>

#### Train

<br>

* Train the sampler

    `bash scprits/train_sampler.sh`

<br>

![Train](/assets/img/2025-12-05/train.png)

<br>

## Grasp Generation & Refinement & Test

<br>

```bash
usage: isaac_test_right.py [-h] [--stability_config STABILITY_CONFIG]
                           --eval_dir EVAL_DIR [--seed SEED] [--device DEVICE]
                           [--onscreen]
```

<br>

---

## References

[1] <https://menggu1234.tistory.com/25>