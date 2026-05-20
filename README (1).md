# Confidence-Aware Curriculum Knowledge Distillation for Resource-Constrained TinyML

This repository presents a thesis-driven implementation of **Confidence-Aware Curriculum Knowledge Distillation (CL-KD)** for TinyML-based hydrophobicity classification on resource-constrained edge devices.[file:9]

The work proposes a two-stage pipeline that first selects hardware-feasible student models and then improves them with confidence-aware curriculum distillation guided by teacher uncertainty.[file:9]

## Overview

The thesis focuses on deploying compact computer-vision models for hydrophobicity classification of composite insulators under strict TinyML constraints such as limited RAM, flash memory, and computation budget.[file:9]

The proposed system combines hardware-aware model design, Monte Carlo dropout-based uncertainty estimation, curriculum learning, and adaptive knowledge distillation into a unified training framework.[file:9]

![TinyML overview](./assets/tinyml-overview.png)

![Hydrophobicity workflow](./assets/hydrophobicity-workflow.png)

## Motivation

Conventional knowledge distillation transfers information from a large teacher model to a small student model, but standard methods use a fixed distillation weight, ignore teacher uncertainty, and do not account for TinyML hardware constraints during student selection.[file:9]

These limitations become more severe for ultra-small models, where unstable supervision can reduce performance instead of improving it.[file:9]

## Core Method

### Stage I: Hardware-Aware Student Design

The first stage formulates student selection as a constrained multi-objective optimization problem that aims to maximize accuracy, minimize flash memory, and satisfy SRAM limits.[file:9]

Pareto-optimal student models are identified using Multi-Objective Bayesian Optimization and passed to the second stage for training.[file:9]

![CL-KD framework](./assets/clkd-two-stage-framework.png)

![Pareto front](./assets/pareto-front.png)

### Stage II: Confidence-Aware Curriculum KD

The second stage estimates teacher confidence using Monte Carlo dropout and uses that confidence to organize samples into easy, medium, and hard subsets for progressive training.[file:9]

A dynamic sample-wise wisdom ratio balances teacher guidance and ground-truth supervision based on student size and confidence level.[file:9]

![Knowledge distillation](./assets/knowledge-distillation.png)

![Confidence-aware KD](./assets/confidence-aware-kd.png)

![Curriculum learning](./assets/curriculum-learning.png)

![MC dropout process](./assets/mc-dropout-process.png)

![MC dropout variations](./assets/mc-dropout-variations.png)

## Background Concepts

The thesis builds on convolutional neural networks, lightweight neural architectures, model compression, curriculum learning, and Pareto optimization for deployment-aware model selection.[file:9]

These ideas together support the design of small models that remain accurate enough for real-world edge inference.[file:9]

![CNN architecture](./assets/cnn-architecture.png)

![Lightweight vs normal NN](./assets/lightweight-vs-normal-nn.png)

![Model size vs accuracy](./assets/model-size-vs-accuracy.png)

## Dataset

The work evaluates the proposed framework on a composite insulator hydrophobicity dataset spanning seven hydrophobicity classes, where subtle visual differences make the task challenging, especially for intermediate classes.[file:9]

The objective is to enable accurate, real-time, deployment-ready hydrophobicity classification for power-system monitoring scenarios.[file:9]

## Experimental Setup

- Framework: TensorFlow / Keras.[file:9]
- GPU: NVIDIA RTX 3050.[file:9]
- CPU: Intel i5 12th Gen.[file:9]
- Batch size: 64.[file:9]
- Optimizer: Adam.[file:9]
- Monte Carlo passes: 50.[file:9]
- Maximum epochs: 300 per curriculum stage, with early stopping.[file:9]

## Reported Results

The thesis reports that CL-KD improves student performance over both baseline training and standard knowledge distillation while maintaining TinyML-friendly model sizes.[file:9]

| Student Model | Flash Size | Baseline Acc. | Standard KD | CL-KD | Gain over KD |
|---|---:|---:|---:|---:|---:|
| Small 1 | 0.77 MB | 69.00 | 78.00 | 89.14 | +11.14 [file:9] |
| Small 2 | 0.09 MB | 76.14 | 70.29 | 86.14 | +15.85 [file:9] |
| Small 3 | 0.43 MB | 78.86 | 70.29 | 84.00 | +13.71 [file:9] |
| Medium 1 | 2.21 MB | 83.29 | 85.43 | 95.57 | +10.14 [file:9] |
| Large 1 | 2.27 MB | 88.29 | 91.57 | 94.67 | +3.10 [file:9] |

The largest gains are reported for ultra-small students, which supports the thesis claim that confidence-aware curriculum distillation is especially useful when student capacity is severely limited.[file:9]

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

## Citation

```bibtex
@mastersthesis{satarupa_das_2026,
  author = {Satarupa Das},
  title = {Confidence-Aware Curriculum-Based Knowledge Distillation for TinyML Applications},
  school = {National Institute of Technology Durgapur},
  year = {2026}
}
```
