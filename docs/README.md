---
layout: home
permalink: index.html

# Please update this with your repository name and title
repository-name: e20-4yp-Self-Supervised-Learning-for-CNN-Transformer-Hybrids-under-Parameter-and-Label-Constraints
title: Self-Supervised-Learning-for-CNN-Transformer-Hybrids
---

[comment]: # "This is the standard layout for the project, but you can clean this and use your own template"

# Project Title

#### Team

- e20259, Munasinghe S.L., [email](mailto:e20259@eng.pdn.ac.lk)

#### Supervisors

- Dr.Sampath Deegalla, [email](mailto:sampath@eng.pdn.ac.lk)

#### Table of content

1. [Abstract](#abstract)
2. [Related works](#related-works)
3. [Methodology](#methodology)
4. [Experiment Setup and Implementation](#experiment-setup-and-implementation)
5. [Results and Analysis](#results-and-analysis)
6. [Conclusion](#conclusion)
7. [Links](#links)

---

<!-- 
DELETE THIS SAMPLE before publishing to GitHub Pages !!!
This is a sample image, to show how to add images to your page. To learn more options, please refer [this](https://projects.ce.pdn.ac.lk/docs/faq/how-to-add-an-image/)
![Sample Image](./images/sample.png) 
-->


## Abstract
Self-supervised learning (SSL) has demonstrated strong representational power for large Vision Transformers trained on massive datasets, yet its applicability to tiny hybrid CNN-Transformer architectures under strict parameter limitations and downstream label-scarce conditions remains largely unexplored. This paper proposes a novel multi-pretext SSL methodology that simultaneously applies token masking and token rotation corruption to the same latent token sequence within a single forward pass.

The methodology introduces three contributions not previously combined: joint, non-overlapping dual corruption, a Dynamic Corruption Scheduler that adapts mask and rotation ratios per step via an exponential moving average (EMA) of per-task loss magnitudes and a gradient-reversal disentanglement loss. Downstream linear evaluation demonstrates that while the pipeline extracts non-trivial semantic features under severe label starvation, it encounters a clear performance boundary compared to server-scale models. We conduct a rigorous failure analysis and isolate three structural failure mechanisms to guide future edge-scale SSL design.


## Related works
- **SSL Pretext Tasks:** Rotation prediction pioneered low-overhead SSL by predicting discrete image orientation updates. MaskFeat and Masked Autoencoders demonstrated that reconstructing masked features or raw pixel patches maps high-quality visual semantics.
- **Multi-Pretext Systems:** Frameworks like ISSS and MAN improve representational richness through joint task fusion, but introduce graph neural networks or complex attention-weighting mechanisms that violate edge-scale low-compute constraints.
- **Tiny Hybrid Architectures:** TinyNeXt introduced efficient hybrid architectures using Lean Single-Head Self-Attention (LeanSHSA) to maximize accuracy-speed performance under 1 M parameters. This research targets the complete absence of SSL methods characterized for these lightweight backbones.


## Methodology
The pipeline is built around an asymmetric encoder-decoder architecture used exclusively during the self-supervised pre-training phase.

### 1. Joint Corruption

Given a token sequence, the methodology applies two non-overlapping corruptions simultaneously. For each sample, token indices are randomly permuted. The first subset of tokens is masked by a learnable embedding, while a disjoint remaining subset is rotated via a cyclic feature-dimension shift:

$$\tilde{t}_i = \text{roll}(t_i, k, \text{dim} = 0), \quad k \in \{1, 2, 3\}$$

### 2. Dynamic Corruption Scheduler

To prevent one pretext task from dominating the gradient path, an EMA-tracked scheduler monitors per-task loss difficulties and balances the token budgets proportionally:

$$r_m = \text{clip}\left(\frac{\hat{\ell}_m}{\hat{\ell}_m + \hat{\ell}_r}, 0.25, 0.5\right)$$

### 3. Disentanglement Loss

To reduce cross-task feature leakage, an adversarial objective using a Gradient Reversal Layer (GRL) penalizes the network if features at masked positions contain orientation indicators, promoting independent representations.

## Experiment Setup and Implementation
The evaluation is conducted on two distinct benchmarks to test scalability and transferability under label-scarce settings.

- **Backbone Configuration:** TinyNeXt-T with feature_dim=192 (approximately 1 M parameters). The shared temporary decoder utilizes a trunk depth of 2 and 8 attention heads.
- **Datasets:** CIFAR-10 (60,000 images) and Tiny-ImageNet (100,000 images across 200 classes).
- **Downstream Protocol:** Following pre-training, the decoder is discarded. The encoder is evaluated via linear evaluation using a highly restricted subset containing only 20% of available downstream labels.


## Results and Analysis
Downstream linear probing evaluation metrics show a performance gap compared to highly scalable, server-grade alternative systems.

| Method                       | CIFAR-10 Accuracy | Tiny-ImageNet Accuracy |
| :--------------------------- | :---------------: | :--------------------: |
| **Proposed Pipeline (Ours)** |      60.01%       |         11.23%         |
| EMP-SSL                      |      91.50%       |         51.50%         |

While the model successfully minimizes both reconstruction and classification losses during pre-training, the optimization of these combined pretext boundaries does not natively map into discriminative features for linear classification tasks.


## Conclusion
This research establishes a concrete empirical boundary condition for multi-pretext self-supervised learning on edge-scale hybrid architectures. The failure is tracked not to conceptual design rules, but to explicit capacity constraints and adversarial interference limits. The actionable remediations characterized—including block-wise spatial masks and projection-based capacity expansion—provide a solid design roadmap for the next generation of lightweight edge-deployable SSL systems.



## Links

[//]: # ( NOTE: EDIT THIS LINKS WITH YOUR REPO DETAILS )

- [Project Repository](https://github.com/cepdnaclk/e20-4yp-Self-Supervised-Learning-for-CNN-Transformer-Hybrids-under-Parameter-and-Label-Constraints)
- [Project Page](https://cepdnaclk.github.io/repository-name)
- [Department of Computer Engineering](http://www.ce.pdn.ac.lk/)
- [University of Peradeniya](https://eng.pdn.ac.lk/)

[//]: # "Please refer this to learn more about Markdown syntax"
[//]: # "https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet"
