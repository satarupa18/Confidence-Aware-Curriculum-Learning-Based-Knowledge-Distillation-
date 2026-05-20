# Confidence-Aware Curriculum Knowledge Distillation for Resource-Constrained TinyML

A research repository for **Confidence-Aware Curriculum Knowledge Distillation (CL-KD)**, a two-stage framework for training high-accuracy, deployment-friendly TinyML models for hydrophobicity classification of composite insulators.[file:1]

This work combines hardware-aware student model design with confidence-driven knowledge distillation so that lightweight models can perform well under tight flash, SRAM, and computation limits.[file:1]

## Overview

The thesis targets image-based hydrophobicity classification on edge devices, where conventional deep models are often too large for direct deployment.[file:1]

The proposed pipeline has two stages:[file:1]

1. **Hardware-aware student selection** using Pareto optimization and Multi-Objective Bayesian Optimization (MOBO).[file:1]
2. **Confidence-aware curriculum knowledge distillation** using teacher uncertainty from Monte Carlo dropout.[file:1]

![TinyML overview](./assets/page-16-figure-1-1.png)

![Hydrophobicity workflow](./assets/page-17-figure-1-2.png)

## Motivation

Standard knowledge distillation helps compress large teacher models into smaller student models, but conventional methods use a fixed distillation weight, assume all samples are equally informative, and ignore teacher uncertainty and hardware constraints during student selection.[file:1]

These limitations are especially harmful for ultra-small TinyML models, where capacity is limited and unstable supervision can significantly degrade performance.[file:1]

## Problem Statement

This work asks how lightweight deep learning models can be designed and trained to achieve high hydrophobicity-classification accuracy while meeting strict TinyML hardware constraints.[file:1]

The optimization problem jointly considers model architecture, knowledge transfer, and training strategy.[file:1]

## Objectives

- Design hardware-efficient student models using Pareto-based optimization.[file:1]
- Develop a confidence-aware curriculum knowledge distillation framework.[file:1]
- Improve the performance of ultra-small TinyML models.[file:1]
- Analyze dataset complexity and its effect on learning.[file:1]
- Support real-time, deployable hydrophobicity classification.[file:1]

## Core Ideas

### 1. Hardware-Aware Student Design

Stage I formulates student selection as a constrained multi-objective optimization problem that aims to maximize accuracy, minimize flash memory, and satisfy SRAM constraints.[file:1]

Pareto-optimal students are selected from the feasible search space and passed to the next training stage.[file:1]

![Pareto and framework](./assets/page-32-figures-4-1-4-2.png)

### 2. Teacher Uncertainty Estimation

Teacher confidence is estimated using Monte Carlo dropout, where multiple stochastic forward passes are performed for each sample and predictive variance is used to derive a confidence score.[file:1]

Low predictive variance corresponds to high confidence, while high variance indicates uncertain teacher predictions.[file:1]

![MC Dropout process](./assets/page-23-figure-2-4.png)

### 3. Confidence-Driven Curriculum Learning

Based on confidence scores, the dataset is partitioned into easy, medium, and hard subsets, and student training progresses from reliable examples to more ambiguous ones.[file:1]

This staged learning improves training stability and helps compact students learn more robustly.[file:1]

![Curriculum learning](./assets/page-23-figure-2-3.png)

### 4. Dynamic Sample-Wise Wisdom Ratio

The CL-KD framework introduces a dynamic sample-wise distillation weight that depends on student size and teacher confidence.[file:1]

Larger students rely more on teacher knowledge, smaller students rely more on ground truth, and low-confidence samples reduce teacher influence.[file:1]

## Method Summary

The final CL-KD training objective balances cross-entropy loss and distillation loss dynamically on a per-sample basis.[file:1]

At a high level, the training workflow is:

1. Train or select a high-capacity teacher model.[file:1]
2. Search and select Pareto-optimal lightweight student architectures.[file:1]
3. Estimate teacher uncertainty using Monte Carlo dropout.[file:1]
4. Split the dataset into curriculum levels based on confidence.[file:1]
5. Train students progressively with the CL-KD loss.[file:1]

## Dataset

The experiments use a publicly available composite insulator hydrophobicity dataset with about 4,500 high-resolution images across seven hydrophobicity classes (HC1-HC7). The dataset includes subtle visual changes in surface wetting behavior, which makes classification difficult, especially for intermediate classes.[file:1]

![Hydrophobicity classes](./assets/page-36-figure-5-1.png)

## Experimental Setup

- Framework: TensorFlow / Keras.[file:1]
- GPU: NVIDIA RTX 3050.[file:1]
- CPU: Intel i5 12th Gen.[file:1]
- Optimizer: Adam.[file:1]
- Batch size: 64.[file:1]
- Maximum epochs: 300 per curriculum stage, with early stopping.[file:1]
- Monte Carlo passes: 50.[file:1]

## Results

The thesis reports that CL-KD improves student performance over both baseline training and standard knowledge distillation, while keeping student models suitable for TinyML settings.[file:1]

| Student Model | Flash Size | Baseline Acc. | Standard KD | CL-KD | Gain over KD |
|---|---:|---:|---:|---:|---:|
| Small 1 | 0.77 MB | 69.00 | 78.00 | 89.14 | +11.14 [file:1] |
| Small 2 | 0.09 MB | 76.14 | 70.29 | 86.14 | +15.85 [file:1] |
| Small 3 | 0.43 MB | 78.86 | 70.29 | 84.00 | +13.71 [file:1] |
| Medium 1 | 2.21 MB | 83.29 | 85.43 | 95.57 | +10.14 [file:1] |
| Large 1 | 2.27 MB | 88.29 | 91.57 | 94.67 | +3.10 [file:1] |

These results indicate that the method is particularly effective for ultra-small models, where standard KD may underperform but CL-KD remains robust.[file:1]

## Why CL-KD Works

The framework works well because it does not treat all samples or teacher outputs equally.[file:1]

Its gains come from four interacting design choices:[file:1]

- Hardware-aware model selection before distillation.[file:1]
- Confidence-aware supervision from the teacher.[file:1]
- Curriculum progression from easy to hard samples.[file:1]
- Dynamic balancing between label supervision and teacher guidance.[file:1]

## Repository Structure

```text
.
├── assets/
├── data/
├── models/
├── notebooks/
├── results/
├── src/
└── README.md
```

## Suggested Usage

```bash
# 1. Prepare dataset
# 2. Train teacher model
# 3. Run Pareto-based student search
# 4. Estimate confidence with MC dropout
# 5. Train students with CL-KD
# 6. Evaluate against baseline and standard KD
```

## Limitations

The thesis notes several current limitations, including the computational overhead of Monte Carlo dropout, quantile-based curriculum partitioning, dependence on a single-teacher setup, and lack of full on-device validation on embedded hardware.[file:1]

## Future Work

Future directions include real TinyML hardware deployment, more efficient uncertainty-estimation methods, multi-teacher distillation, adaptive curriculum schedules, and evaluation on other domains such as medical imaging and industrial inspection.[file:1]

## Citation

```bibtex
@mastersthesis{satarupa_das_2026,
  author = {Satarupa Das},
  title = {Confidence-Aware Curriculum Knowledge Distillation for Resource-Constrained TinyML},
  school = {National Institute of Technology Durgapur},
  year = {2026},
  month = {May}
}
```
