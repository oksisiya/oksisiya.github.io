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

```bash
# ~/DexDiffuser
.
├── configs
│   ├── default.yaml
│   ├── diffuser
│   │   └── ddpm.yaml
│   ├── model
│   │   ├── unet_grasp_bps.yaml
│   │   └── unet_grasp_pn2.yaml
│   ├── sample.yaml
│   └── task
│       ├── evaluator_dexgn_slurm.yaml
│       └── grasp_gen_ur_dexgn_slurm.yaml
├── dataset
│   ├── all_obj.txt
│   ├── evaluator_dataset.py
│   ├── __init__.py
│   ├── misc.py
│   ├── sampler_dataset.py
│   ├── test_split.txt
│   └── train_split.txt
├── envs
│   ├── assets
│   │   └── movable_hand_urdf
│   │       ├── allegro
│   │       │   ├── meshes
│   │       │   │   ├── allegro
│   │       │   │   │   ├── base_link.obj
│   │       │   │   │   ├── base_link.STL
│   │       │   │   │   ├── primary_base.obj
│   │       │   │   │   ├── primary_base.stl
│   │       │   │   │   ├── primary_medial.obj
│   │       │   │   │   ├── primary_medial.stl
│   │       │   │   │   ├── primary_proximal.obj
│   │       │   │   │   ├── primary_proximal.stl
│   │       │   │   │   ├── thumb_base.obj
│   │       │   │   │   ├── thumb_base.stl
│   │       │   │   │   ├── thumb_medial.obj
│   │       │   │   │   ├── thumb_medial.stl
│   │       │   │   │   ├── thumb_proximal.obj
│   │       │   │   │   └── thumb_proximal.stl
│   │       │   │   ├── base_link_left.STL
│   │       │   │   ├── base_link.STL
│   │       │   │   ├── biotac
│   │       │   │   │   ├── biotac_sensor.obj
│   │       │   │   │   ├── biotac_sensor.stl
│   │       │   │   │   ├── biotac_sensor_thumb.obj
│   │       │   │   │   └── biotac_sensor_thumb.stl
│   │       │   │   ├── iiwa7
│   │       │   │   │   ├── collision
│   │       │   │   │   │   ├── link_0.obj
│   │       │   │   │   │   ├── link_0.stl
│   │       │   │   │   │   ├── link_1.obj
│   │       │   │   │   │   ├── link_1.stl
│   │       │   │   │   │   ├── link_2.obj
│   │       │   │   │   │   ├── link_2.stl
│   │       │   │   │   │   ├── link_3.obj
│   │       │   │   │   │   ├── link_3.stl
│   │       │   │   │   │   ├── link_4.obj
│   │       │   │   │   │   ├── link_4.stl
│   │       │   │   │   │   ├── link_5.obj
│   │       │   │   │   │   ├── link_5.stl
│   │       │   │   │   │   ├── link_6.obj
│   │       │   │   │   │   ├── link_6.stl
│   │       │   │   │   │   ├── link_7.obj
│   │       │   │   │   │   ├── link_7_old.obj
│   │       │   │   │   │   ├── link_7_old.stl
│   │       │   │   │   │   └── link_7.stl
│   │       │   │   │   └── visual
│   │       │   │   │       ├── link_0.obj
│   │       │   │   │       ├── link_0.stl
│   │       │   │   │       ├── link_1.obj
│   │       │   │   │       ├── link_1.stl
│   │       │   │   │       ├── link_2.obj
│   │       │   │   │       ├── link_2.stl
│   │       │   │   │       ├── link_3.obj
│   │       │   │   │       ├── link_3.stl
│   │       │   │   │       ├── link_4.obj
│   │       │   │   │       ├── link_4.stl
│   │       │   │   │       ├── link_5.obj
│   │       │   │   │       ├── link_5.stl
│   │       │   │   │       ├── link_6.obj
│   │       │   │   │       ├── link_6.stl
│   │       │   │   │       ├── link_7.obj
│   │       │   │   │       ├── link_7_old.obj
│   │       │   │   │       ├── link_7_old.stl
│   │       │   │   │       └── link_7.stl
│   │       │   │   ├── link_0.0.STL
│   │       │   │   ├── link_1.0.STL
│   │       │   │   ├── link_12.0_left.STL
│   │       │   │   ├── link_12.0_right.STL
│   │       │   │   ├── link_13.0.STL
│   │       │   │   ├── link_14.0.STL
│   │       │   │   ├── link_15.0.STL
│   │       │   │   ├── link_15.0_tip.STL
│   │       │   │   ├── link_2.0.STL
│   │       │   │   ├── link_3.0.STL
│   │       │   │   ├── link_3.0_tip.STL
│   │       │   │   ├── link_4.0.STL
│   │       │   │   └── mounts
│   │       │   │       ├── allegro_mount.obj
│   │       │   │       └── allegro_mount.stl
│   │       │   └── robots
│   │       │       ├── allegro_hand_description_left.urdf
│   │       │       ├── allegro_hand_description_right.urdf
│   │       │       ├── movable_allegro_backup.urdf
│   │       │       ├── movable_allegro-copy.urdf
│   │       │       └── movable_allegro.urdf
│   │       └── allegro_isaac
│   │           ├── allegro.urdf
│   │           ├── meshes
│   │           │   ├── allegro
│   │           │   │   ├── base_link.obj
│   │           │   │   ├── base_link.STL
│   │           │   │   ├── primary_base.obj
│   │           │   │   ├── primary_base.stl
│   │           │   │   ├── primary_medial.obj
│   │           │   │   ├── primary_medial.stl
│   │           │   │   ├── primary_proximal.obj
│   │           │   │   ├── primary_proximal.stl
│   │           │   │   ├── thumb_base.obj
│   │           │   │   ├── thumb_base.stl
│   │           │   │   ├── thumb_medial.obj
│   │           │   │   ├── thumb_medial.stl
│   │           │   │   ├── thumb_proximal.obj
│   │           │   │   └── thumb_proximal.stl
│   │           │   ├── biotac
│   │           │   │   ├── biotac_sensor.obj
│   │           │   │   ├── biotac_sensor.stl
│   │           │   │   ├── biotac_sensor_thumb.obj
│   │           │   │   └── biotac_sensor_thumb.stl
│   │           │   ├── convert_stl2obj.py
│   │           │   ├── iiwa7
│   │           │   │   ├── collision
│   │           │   │   │   ├── link_0.obj
│   │           │   │   │   ├── link_0.stl
│   │           │   │   │   ├── link_1.obj
│   │           │   │   │   ├── link_1.stl
│   │           │   │   │   ├── link_2.obj
│   │           │   │   │   ├── link_2.stl
│   │           │   │   │   ├── link_3.obj
│   │           │   │   │   ├── link_3.stl
│   │           │   │   │   ├── link_4.obj
│   │           │   │   │   ├── link_4.stl
│   │           │   │   │   ├── link_5.obj
│   │           │   │   │   ├── link_5.stl
│   │           │   │   │   ├── link_6.obj
│   │           │   │   │   ├── link_6.stl
│   │           │   │   │   ├── link_7.obj
│   │           │   │   │   ├── link_7_old.obj
│   │           │   │   │   ├── link_7_old.stl
│   │           │   │   │   └── link_7.stl
│   │           │   │   └── visual
│   │           │   │       ├── link_0.obj
│   │           │   │       ├── link_0.stl
│   │           │   │       ├── link_1.obj
│   │           │   │       ├── link_1.stl
│   │           │   │       ├── link_2.obj
│   │           │   │       ├── link_2.stl
│   │           │   │       ├── link_3.obj
│   │           │   │       ├── link_3.stl
│   │           │   │       ├── link_4.obj
│   │           │   │       ├── link_4.stl
│   │           │   │       ├── link_5.obj
│   │           │   │       ├── link_5.stl
│   │           │   │       ├── link_6.obj
│   │           │   │       ├── link_6.stl
│   │           │   │       ├── link_7.obj
│   │           │   │       ├── link_7_old.obj
│   │           │   │       ├── link_7_old.stl
│   │           │   │       └── link_7.stl
│   │           │   └── mounts
│   │           │       ├── allegro_mount.obj
│   │           │       └── allegro_mount.stl
│   │           └── movable_allegro.urdf
│   ├── base_task.py
│   ├── __init__.py
│   └── tasks
│       ├── base_task.py
│       ├── grasp_test_force_allegro.py
│       ├── grasp_test_force_barrett.py
│       ├── grasp_test_force_ezgripper.py
│       ├── grasp_test_force_shadowhand.py
│       ├── grasp_test_force.yaml
│       ├── __init__.py
│       └── utils
│           └── angle.py
├── imgs
│   ├── pull_image.png
│   └── sim.png
├── isaac_test_right.py
├── LICENSE
├── models
│   ├── basis_point_set.npy
│   ├── dm
│   │   ├── ddpm.py
│   │   └── schedule.py
│   ├── __init__.py
│   └── model
│       ├── evaluator.py
│       ├── __init__.py
│       ├── pointnet2
│       │   ├── pointnet2_modules.py
│       │   ├── pointnet2_semseg.py
│       │   ├── pointnet2_utils.py
│       │   └── pytorch_utils.py
│       ├── unet.py
│       ├── utils.py
│       └── visualizer.py
├── README.md
├── refine.py
├── sample.py
├── scripts
│   ├── refine.sh
│   ├── sample.sh
│   ├── train_evaluator.sh
│   ├── train_sampler_ddm.sh
│   └── train_sampler.sh
├── train_ddm.py
├── train.py
└── utils
    ├── handmodel.py
    ├── io.py
    ├── plotly_utils.py
    ├── plot.py
    ├── rot6d.py
    └── utils.py

34 directories, 191 files

```

<br>

> ## [25/12/08] DexDiffuser 환경 구축 [✗]
> * README.md에 명시된 대로 dexdiff 환경 구성
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

> ## [25/12/10] DexDiffuser 환경 구축 [✓]
> * 

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


## Train

* Train the sampler

    `bash scprits/train_sampler.sh`

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