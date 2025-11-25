---
title: "DexDiffuser: Generating Dexterous Grasps with Diffusion Models (6 Nov 2024)"
date: 2025-11-25 15:39:00 +0900
categories: [Review]
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