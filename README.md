# Self-Supervised Learning for CNN-Transformer Hybrids under Parameter and Label Constraints

---

## Project Introduction
This project introduces a lightweight, patch-level, multi-pretext self-supervised learning (SSL) framework engineered specifically for resource-constrained architectures. While standard vision models require high parametric capacity, our methodology optimizes representation learning for a compact **1-Million parameter hybrid CNN-Transformer backbone (TinyNeXt-T)** under severe downstream label starvation. 

The core pipeline simultaneously applies non-overlapping token masking and token rotation corruptions within a single forward pass, dynamically balancing task workloads using an Exponential Moving Average (EMA) scheduler and isolating cross-task feature leakage via an adversarial gradient-reversal disentanglement loss.



## Key Project Links
* **Institutional Affiliation:** [Department of Computer Engineering, University of Peradeniya](http://www.ce.pdn.ac.lk/)
