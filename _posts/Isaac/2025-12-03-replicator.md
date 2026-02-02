---
title: "Isaac Sim Replicator의 개념과 사용"
date: 2025-12-03 11:42:00 +0900
categories: [Isaac]
---

---

&nbsp;

## Replicator

<br>

* Isaac Sim에서 합성 데이터를 생성하기 위한 프레임워크
* 도메인 랜덤화를 통해 수천 장의 학습 데이터를 자동으로 생성할 수 있음

<br>

#### 라이브러리

<br>

```python
import omni.replicator.core as rep
import omni.usd
from isaacsim.core.utils.semantics import add_update_semantics
from pxr import Sdf, UsdGeom
```

<br>

* `import omni.replicator.core as rep`
    * Replicator의 메인 API

* `add_update_semantics`
    * 객체의 semantic 레이블을 추가하는 함수
    * Segmentation mask 생성에 반드시 필요함

<br>

### 새로운 스테이지 생성

<br>

```python
# 새 스테이지 생성
omni.usd.get_context().new_stage()
stage = omni.usd.get_context().get_stage()

# Replicator capture on play 비활성화 (수동 제어)
rep.orchestrator.set_capture_on_play(False)
```

<br>

* `set_capture_on_play()`
    * 자동 캡처를 끄고 수동으로 제어
    * 정확한 타이밍에 데이터 생성 가능

<br>

### 환경 구성

<br>

```python
# 테이블 (큐브로 표현)
table = stage.DefinePrim("/World/Table", "Cube")
UsdGeom.Xformable(table).AddTranslateOp().Set((0.0, 0.0, 0.4))
UsdGeom.Xformable(table).AddScaleOp().Set((0.8, 0.8, 0.05))
UsdGeom.Gprim(table).CreateDisplayColorAttr([(0.6, 0.4, 0.2)])
add_update_semantics(table, "table")
```

<br>

이 코드는 환경을 구성하는 것으로 usd api로 직접 prim을 생성한다.

* `DefinePrim()`: 큐브를 만들고

* `Xformable()`: 로 변환을 적용한다.
    
    * `AddTranslateOp()`: 이동
    * `AddScaleOp()`: 크기 조절
    
* `add_update_semantics()`: table 레이블을 추가해서 segmantation에서 구분할 수 있게 한다.

<br>

#### 학습용 객체 생성

<br>

```python
# 객체 1: 빨간 큐브
cube1 = stage.DefinePrim("/World/Cube1", "Cube")
UsdGeom.Xformable(cube1).AddTranslateOp().Set((0.0, 0.0, 0.5))
UsdGeom.Xformable(cube1).AddScaleOp().Set((0.05, 0.05, 0.05))
UsdGeom.Gprim(cube1).CreateDisplayColorAttr([(1.0, 0.0, 0.0)])
add_update_semantics(cube1, "red_cube")

# 객체 2: 초록 구
sphere1 = stage.DefinePrim("/World/Sphere1", "Sphere")
UsdGeom.Xformable(sphere1).AddTranslateOp().Set((0.15, 0.0, 0.5))
UsdGeom.Xformable(sphere1).AddScaleOp().Set((0.05, 0.05, 0.05))
UsdGeom.Gprim(sphere1).CreateDisplayColorAttr([(0.0, 1.0, 0.0)])
add_update_semantics(sphere1, "green_sphere")

# 객체 3: 파란 실린더
cylinder1 = stage.DefinePrim("/World/Cylinder1", "Cylinder")
UsdGeom.Xformable(cylinder1).AddTranslateOp().Set((-0.15, 0.0, 0.5))
UsdGeom.Xformable(cylinder1).AddScaleOp().Set((0.05, 0.05, 0.1))
UsdGeom.Gprim(cylinder1).CreateDisplayColorAttr([(0.0, 0.0, 1.0)])
add_update_semantics(cylinder1, "blue_cylinder")
```

<br>

이 코드는 학습용 객체 생성을 하는 코드이다. 세 가지 기본 형태를 만들어 학습 데이터를 생성한다.

* `CreateDisplayColorAttr()`: 색상을 설정하고 각각 다른 semantic 레이블을 부여한다. 이 레이블이 나중에 segmantation mask의 클래스가 된다.

* `add_update_semantics()`: red_cube라는 이름표를 붙이면 replicator는 나중에 segmantation 데이터를 생성할 때 이 이름표가 붙은 객체의 픽셀들을 약속된 클래스 아이디로 모두 칠해준다.

<br>

#### 랜덤화

<br>

```python
# 색상 랜덤화 함수
def randomize_colors():
    """객체들의 색상을 랜덤하게 변경"""
    cube1_prim = rep.get.prim_at_path("/World/Cube1")
    sphere1_prim = rep.get.prim_at_path("/World/Sphere1")
    cylinder1_prim = rep.get.prim_at_path("/World/Cylinder1")

    with cube1_prim:
        rep.randomizer.color(colors=rep.distribution.uniform((0.5, 0, 0), (1, 0.3, 0.3)))
    with sphere1_prim:
        rep.randomizer.color(colors=rep.distribution.uniform((0, 0.5, 0), (0.3, 1, 0.3)))
    with cylinder1_prim:
        rep.randomizer.color(colors=rep.distribution.uniform((0, 0, 0.5), (0.3, 0.3, 1)))

    return cube1_prim.node
```

<br>

* `randomize_colors()`: 색상 렌덤화 함수이다. 도메인 랜덤화의 핵심인 색상 변경 함수이다.

* `rep.get.prim_at_path()`: 이 함수를 통해 객체를 가져오고 with 문 안에서 램덤화를 적용한다.

* `rep.distribution.uniform()`: 이 함수로 균등 분포로 빨간색 계열 내에서 랜덤하게 색상을 선택한다. 이렇게 하면 다양한 조명 조건을 시뮬레이션 할 수 있다.

<br>

```python
# 위치 랜덤화 함수
def randomize_positions():
    """객체들의 위치를 테이블 위에서 랜덤하게 변경"""
    objects = rep.get.prims(path_pattern="/World/(Cube1|Sphere1|Cylinder1)")
    with objects:
        rep.modify.pose(
            position=rep.distribution.uniform((-0.25, -0.25, 0.45), (0.25, 0.25, 0.52)),
            rotation=rep.distribution.uniform((0, 0, 0), (0, 0, 360))
        )
    return objects.node
```

<br>

* `randomize_positions()`: 위치 랜덤화 함수이다. 위치와 회전을 랜덤화 한다. 이 함수로 정규 표현식을 사용해 여러 객체를 한 번에 선택한다.

* `position=rep.distribution.uniform()`: 테이블 위 범위 내에서 rotation은 z축 회전만 0부터 360도로 설정한다. 이렇게 하면 객체가 다양한 위치와 방향에서 학습되도록 데이터를 생성할 수 있다.

<br>

#### 랜덤화 함수 등록

<br>

```python
# 랜덤화 함수 등록
rep.randomizer.register(randomize_colors)
rep.randomizer.register(randomize_positions)

# 매 프레임마다 랜덤화 실행
with rep.trigger.on_frame():
    rep.randomizer.randomize_colors()
    rep.randomizer.randomize_positions()
```

<br>

* `rep.randomizer.register()`: 랜덤화 함수를 등록하고

* `rep.trigger.on_frame()`: 매 frame마다 실행되도록 설정할 수 있다. 이렇게 하면 각 프레임이 다른 설정을 가지게 데이터의 다양성을 확보할 수 있다. trigger_on_frame은 replicator가 데이터가 생성하는 모든 프레임마다 랜덤화 함수를 실행하는 강력한 명령어이다. 이를 통해 프레임별로 완전히 독립적인 학습 데이터를 생성할 수 있다.

<br>

#### Render Product와 Writer 설정

<br>

```python
# Render Product 생성 (카메라와 해상도 연결)
render_product = rep.create.render_product("/World/Camera", (512, 512))

# 출력 디렉토리 설정
output_dir = os.path.join(os.path.dirname(__file__), "replicator_output", "lecture03_basic")
os.makedirs(output_dir, exist_ok=True)

# Writer 설정 (문서와 일치하는 어노테이션만 생성)
writer = rep.writers.get("BasicWriter")
writer.initialize(
    output_dir=output_dir,
    rgb=True,  # RGB 이미지 캡처
    bounding_box_2d_tight=True,  # 2D Bounding Box
    semantic_segmentation=True,  # Semantic Segmentation 기초
    distance_to_image_plane=True  # Depth 맵 생성
)
writer.attach(render_product)
```

<br>

이 코드는 render_product와 writer를 설정한다.

* `render_product`: 카메라와 해상도를 연결하는 객체이다.

* `writer`: 데이터를 저장하는 컴포넌트로 어떤 어노테이션을 생성할지 설정한다. rgb 이미지, 2d 바운딩 박스, 세그멘테이션 마스크, 그리고 깊이 맵을 설정하도록 설정했다.

* `attach()`: 카메라와 연결한다

<br>

#### ...

<br>

```python
# 데이터 캡처 실행
for i in range(args.num_frames):
    print(f"프레임 {i+1}/{args.num_frames} 생성 중...", end='\r')
    rep.orchestrator.step()

print(f"\n✓ {args.num_frames}개 프레임 생성 완료")

# Writer 정리
writer.detach()

# 데이터 쓰기 완료 대기
print("데이터 저장 중...")
rep.orchestrator.wait_until_complete()
```

<br>

* `rep.orchestrator.step()`: 한 프레임을 생성할 때 사용하는 코드이다. 각 프레임마다 랜덤화가 적용되어 다른 데이터가 생성된다.

* `detach()`: writer를 분리한다.

* `wait_until_complete()`: 모든 데이터가 디스크에 저장될 때까지 기다린다.

<br>

### ...

<br>

```python
# 생성된 파일 확인
if os.path.exists(output_dir):
    import glob
    rgb_files = glob.glob(os.path.join(output_dir, "*rgb*.png"))
    seg_files = glob.glob(os.path.join(output_dir, "*semantic*.png"))
    bbox_files = glob.glob(os.path.join(output_dir, "*bounding*.npy"))

    print(f"✓ RGB 이미지: {len(rgb_files)}개")
    print(f"✓ Segmentation 마스크: {len(seg_files)}개")
    print(f"✓ Bounding Box: {len(bbox_files)}개")
    print(f"✓ 저장 위치: {output_dir}")
```

<br>

이 코드는 생성된 파일을 확인한다. rgb 이미지는 png 형식. 바운딩 박스는 numpy 배열로 저장된다. segmantation 마스크는 각 픽셀이 클래스 아이디를 나타내는 단일 채널 이미지이다.

<br>

## 스크립트 실행 결과 확인

<br>

스크립트가 존재하는 폴더에서 아래의 코드를 실행한다,.

<br>

```
. ~/isaac-sim/python.sh replicator.py
```

<br>

스크립트의 실행 결과를 확인한다. 시뮬레이터가 실행되고 매우 빠른 속도로 데이터가 생성되는 것을 확인할 수 있다. 폴더에서 다양한 위치와 로테이션이 적용된 rgb 이미지들을 확인할 수 있다. semantic segmantation 데이터도 확인할 수 있다. 그리고 각 레이블 파일들이 numpy 형태(.npy)로 그리고 json 형태로 저장되어 있는 것을 확인할 수 있다. 

<br>

## Summary

<br>

아래의 다섯 가지 요소가 조합되어 자동화된 데이터 생성 파이프라인을 구성한다.

* `rep.create.render_product()`: 카메라와 해상도를 연결
* `rep.writers.get`: 데이터 저장과 관련된 설정
* `rep.randomizer.register()`: 도메인 랜덤화 (랜덤화 함수 등록)
* `rep.trigger.on_frame()`: 
* `rep.orchestrator.step()`: 한 프레임을 시뮬레이션

<br>

---

## References

[1] <https://docs.isaacsim.omniverse.nvidia.com/4.2.0/replicator_tutorials/tutorial_replicator_getting_started.html>