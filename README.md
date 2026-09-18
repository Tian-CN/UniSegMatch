# UniSegMatch

**Official implementation of UniSegMatch for semi-supervised pleural effusion segmentation in ultrasound, combining large vision model (LVM)-guided pseudo-labeling, consistency learning, and shift-window state-space modeling.**

> **Release status:** This repository currently provides the core **training and validation code** used for the UniSegMatch experiments on **E-FAST-II** and **ISIC2017**. Additional code for the complete LVM-based pseudo-label generation pipeline, reproduction utilities, and data-related materials is being整理ed and will be released in subsequent updates.

## Overview

**UniSegMatch** is a semi-supervised medical image segmentation framework developed for pleural effusion segmentation in trauma ultrasound.

The framework integrates complementary supervision from pretrained large vision models with semi-supervised consistency learning and state-space modeling. Rather than treating the individual components as standalone contributions, UniSegMatch focuses on their integration for medical image segmentation under limited labeled data.

The overall framework contains three main components:

1. **LVM-guided Pseudo-Label Generator (P-L Generator)**
   SegGPT and SAM-Med2D are used as complementary pretrained vision models to generate pseudo supervision for unlabeled images. Their outputs are subsequently combined through reliability-aware processing and pseudo-label fusion.

2. **US-Match Semi-Supervised Learning**
   UniSegMatch combines labeled supervision, LVM-derived pseudo supervision, and weak-to-strong consistency learning. Agreement, boundary information, and prediction confidence are incorporated to reduce the influence of unreliable pseudo supervision.

3. **US-SSM Segmentation Network**
   A shift-window state-space modeling strategy is introduced to enhance local and long-range feature interaction while retaining the computational advantages of state-space models.

The framework is primarily evaluated on **E-FAST-II ultrasound images for pleural effusion segmentation**, with additional experiments on **ISIC2017** to evaluate its applicability beyond ultrasound imaging.

---

## Current Code Release

The current repository release focuses on the code required for:

* UniSegMatch model training;
* validation and evaluation;
* E-FAST-II experiments;
* ISIC2017 experiments;
* the main UniSegMatch training configuration;
* the segmentation network and semi-supervised learning components used in the reported experiments.

The repository is being released progressively. The following materials are still being cleaned and organized and will be added in future updates:

* complete LVM pseudo-label generation pipeline;
* SegGPT prompt construction and pseudo-label generation utilities;
* SAM-Med2D prompt construction and pseudo-label generation utilities;
* additional pseudo-label processing and analysis tools;
* scripts for reproducing individual ablation experiments;
* consolidated environment and dependency specifications;
* pretrained-model/checkpoint instructions where applicable;
* additional dataset preparation utilities;
* data-access documentation and processed data examples where release is permitted.

This staged release is intended to avoid publishing historical development scripts, duplicated versions, or experiment-specific files before their provenance and correspondence with the reported experiments have been fully verified.

---

## Large Vision Models

UniSegMatch uses pretrained large vision models as external components for LVM-guided pseudo-label generation.

### SAM-Med2D

We use **SAM-Med2D** for medical image segmentation with prompt-based inference:

[uni-medical/SAM-Med2D](https://github.com/uni-medical/SAM-Med2D)

> SAM-Med2D: Bridging the Gap between Natural Image Segmentation and Medical Image Segmentation

Please follow the original repository for model checkpoints, dependencies, licensing requirements, and citation information.

### SegGPT / Painter

SegGPT is obtained from the **Painter & SegGPT** repository maintained by BAAI Vision:

[baaivision/Painter](https://github.com/baaivision/Painter)

> Painter & SegGPT Series: Vision Foundation Models from BAAI

Please follow the original repository for SegGPT checkpoints, inference dependencies, licensing requirements, and citation information.

The pretrained LVMs are used as external pseudo-label generators in the UniSegMatch pipeline. Users should download and configure their corresponding implementations and pretrained weights according to the instructions provided by the original repositories.

---

## Method Pipeline

The conceptual UniSegMatch pipeline can be summarized as:

```text
                         Labeled images
                              │
                              ▼
                     Supervised learning
                              │
                              │
Unlabeled images ──► LVM-guided P-L Generator
                     │              │
                  SegGPT        SAM-Med2D
                     │              │
                     └──────┬───────┘
                            ▼
                 Pseudo-label processing
                 / reliability filtering
                            │
                            ▼
                Pseudo supervision
                            │
                            ├──────────────┐
                            │              │
                            ▼              ▼
                     Weak prediction   Strong prediction
                            │              │
                            └──────┬───────┘
                                   ▼
                         Consistency learning
                                   │
                                   ▼
                         UniSegMatch / US-SSM
                                   │
                                   ▼
                         Segmentation output
```

The LVM-based pseudo-label generation stage is performed separately from the subsequent UniSegMatch student-model training.

---

## Datasets

### E-FAST-II

E-FAST-II is used as the primary dataset for evaluating semi-supervised pleural effusion segmentation in trauma ultrasound.

Because the dataset contains medical ultrasound data, its public release and access procedure are being handled separately from the source-code release. Dataset-related information will be updated after the corresponding materials and release conditions are finalized.

### ISIC2017

The **ISIC2017 skin-lesion segmentation dataset** is used as an external dataset to evaluate UniSegMatch outside the ultrasound domain.

Users should obtain the original ISIC2017 data from the official dataset source and comply with its corresponding terms of use.

Dataset paths are intentionally not hard-coded into this README because the final public data-directory convention is still being consolidated.

---

## Installation

The current code package contains the implementation used for training and validation. A consolidated and verified environment specification will be provided after the dependency cleanup is completed.

Because several historical development environments and LVM dependencies were used during the project, we do **not** provide unverified package versions in this preliminary README.

The final release will document:

```text
Python version
PyTorch / CUDA versions
Core Python dependencies
State-space-model dependencies
SAM-Med2D environment
SegGPT environment
Pretrained checkpoints
Dataset directory structure
```

For SAM-Med2D and SegGPT themselves, please follow the installation instructions in their respective upstream repositories.

---

## Training and Evaluation

Training and validation implementations for the currently released E-FAST-II and ISIC2017 experiments are included in this repository.

A standardized command-line interface and final one-command reproduction instructions will be added after the repository structure, configuration files, and experiment naming conventions are fully consolidated.

We intentionally avoid documenting historical script names or development-version commands here when their correspondence to the final reported experiments has not yet been fully verified.

This is particularly important because several intermediate versions were created during method development and ablation experiments. The public repository will retain the reproducible implementation associated with the final method while separating or removing historical development artifacts.

---

## Reproducibility

Our goal is to provide sufficient materials to reproduce the principal experiments reported for UniSegMatch.

The complete reproducibility release is planned to include:

* final training configurations;
* validation/evaluation protocol;
* dataset preparation instructions;
* subject-level data splitting procedure where applicable;
* LVM pseudo-label generation configuration;
* pseudo-label processing settings;
* pretrained-model information;
* main-method and ablation configurations;
* random-seed and repeated-experiment settings where applicable;
* scripts for aggregating evaluation results.

Because the source package is currently undergoing final cleanup, only settings that can be verified against the experiments will be documented as definitive.

---

## Repository Status

| Component                                | Status                |
| ---------------------------------------- | --------------------- |
| UniSegMatch core implementation          | ✅ Available           |
| E-FAST-II training code                  | ✅ Available           |
| E-FAST-II validation code                | ✅ Available           |
| ISIC2017 training code                   | ✅ Available           |
| ISIC2017 validation code                 | ✅ Available           |
| US-Match implementation                  | ✅ Available           |
| US-SSM implementation                    | ✅ Available           |
| Complete SegGPT pseudo-label pipeline    | 🚧 Being organized    |
| Complete SAM-Med2D pseudo-label pipeline | 🚧 Being organized    |
| Unified experiment configurations        | 🚧 Being organized    |
| Environment specification                | 🚧 Being consolidated |
| Complete ablation reproduction scripts   | 🚧 Being organized    |
| Dataset release/access documentation     | 🚧 In preparation     |
| Additional processed data/materials      | 🚧 In preparation     |

---

## Citation

This repository accompanies the manuscript:

**LVM-Guided Semi-Supervised Segmentation of Pleural Effusion in Trauma Ultrasound via State Space Modeling**

The formal citation and BibTeX entry will be added after the publication information is finalized.

If you use the LVM components, please also cite the corresponding **SAM-Med2D** and **SegGPT/Painter** works according to their original repositories.

---

## Acknowledgements

UniSegMatch builds upon and benefits from several excellent open-source projects in medical image segmentation and vision foundation models.

We particularly acknowledge:

* **SAM-Med2D**
  https://github.com/uni-medical/SAM-Med2D
  for providing a medical-domain adaptation of the Segment Anything framework used in our LVM-guided pseudo-label generation pipeline.

* **Painter / SegGPT**
  https://github.com/baaivision/Painter
  for providing the SegGPT implementation used as another complementary large vision model for pseudo-label generation.

* **VM-UNet**
  https://github.com/JCruan519/VM-UNet
  for its open-source implementation of Vision Mamba-based U-Net architectures for medical image segmentation, which provided valuable reference for the design and implementation of our state-space-model-based segmentation network.

We sincerely thank the authors and maintainers of these projects for making their research and implementations publicly available.

Please refer to the original repositories for their respective licenses and citation requirements.

---

## Updates

This repository is under active整理 and will be updated as additional reproducibility materials are finalized.

Future updates will focus on completing the full pseudo-label generation pipeline, consolidating experiment configurations, documenting data preparation, and providing a clearer end-to-end reproduction workflow.

Please check the repository for subsequent updates.
