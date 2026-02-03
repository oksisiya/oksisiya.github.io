---
title: "DexDiffuser: Generating Dexterous Grasps with Diffusion Models (6 Nov 2024)"
date: 2025-11-25 15:39:00 +0900
categories: [Review]

published: false
use_math: true
---
---

&nbsp;

## Abstract

<br>

**DexDiffuser**

A dexterous grasping method that <u>generates</u>, <u>evaluates</u>, and <u>refines</u> grasps on partial object point clouds
* DexSampler: the conditional diffusion-based grasp sampler
* DexEvaluator: the dexterous grasp evaluator

<br>

**DexSampler**

Generates high-quality grasps conditioned on object point clouds by iterative denoising of randomly sampled grasps

<br>

**Grasp Refinement Strategies**
* Evaluator-Guided Diffusion
* Evaluator-based Sampling Refinement

<br>

**Experiment Results**

Demonstrate that DexDiffuser consistently outperforms the state-of-the-art multi-finger grasp generation method FFHNet with, an, on average, 9.12% and 19.44% higher grasp success rate in simulation and real robot experiments, respectively

<br>

## Introduction

<br>

**Generating Dexterous Grasps**
* 데이터 기반 파지(data-driven grasping)에 대한 수년간의 연구에도 불구하고, 알려지지 않은 물체를 집기 위한 섬세한 파지(dexterous grasps)를 생성하는 것은 여전히 어려운 과제이다.
* Dexterous grasping의 주요 과제 중 하나는 고차원의 탐색 공간(high-dimensional search space)에서 성공적인 파지를 식별하는(identifying) 것이다.

<br>

**DexDiffuser**
* 본 논문에서는 차원 문제(dimensionality issue)를 해결하기 위해 데이터 기반 섬세한 파지 기법(data-driven dexterous grasping method)인 DexDiffuser를 제안한다.
* DexDiffuser는 DexSampler와 DexEvaluator를 포함한다.
    * **DexSampler**: a new conditional diffusion-based dexterous grasp sampler
    * **DexEvaluator**: a new dexterous grasp evaluator

<br>

**DexSampler**

DexSampler는 객체 포인트 클라우드(object point clouds)를 조건으로 무작위로 샘플링된 파지의 노이즈를 반복적으로 제거해 고품질 파지를 생성한다.

<br>

**Train the Models**

<br>

**Two Grasp Refinement Strategies**

본 논문에서는 DexDiffuser와 함께 두 가지의 파지 개선(grasp refinement) 기법을 제안한다.
* **Evaluator-Guided Diffusion (EGD)**: only applicable for diffusion models
* **Evaluator-based Sampling Refinement (ESR)**: applies to all dexterous grasp sampling methods

<br>

## Method

<br>

#### The Basis Point Set Representation

<br>

**Basis Point Set (BPS)**
* BPS 

<br>

## Conclusion

<br>

**Problem of This Paper**

Dexterous grasping under partial point cloud observations

<br>

**DexDiffuser**
* Consists of a conditional diffusion-based grasp sampler and grasp evaluator, BPS-encoded point clouds, and two different grasp refinement strategies
* Data-driven dexterous grasping methods that generate grasps directly on partial point clouds and is evaluated on real hardware

<br>

**Experiments on the 16-DoF Allegro Hand**

Demonstrated that DexDiffuser outperformed FFHNet with an average of 9.12% and 19.44% higher grasp success rate in simulation and real robot experiments, respectively

<br>

**Limitations of DexDiffuser**
* Sim-to-real gap
* Relatively long grasp sampling time

<br>

---

## References

[1] <https://arxiv.org/abs/2402.02989> (paper)  
[2] <https://yulihn.github.io/DexDiffuser_page/> (supplementary materials)