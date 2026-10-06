---
title: VLM MLP Projection
---

*Placeholder content - original file not found in upstream repository.*
# 4.3 VLM: MLP and Linear Feature Projection
{: #lesson-vlm-mlp-projection}

## Overview
A pretrained vision encoder processes image patches into feature tokens. A multi-layer perceptron (MLP) or linear projection maps these patch vectors into the input token embedding space of a large language model.

## Lesson Narrative
In this lesson we explore how to bridge a vision backbone with a language model using a learned projection network. The vision encoder (e.g., a small CLIP or ViT model) outputs a sequence of patch embeddings of shape *(N, D)*. The language model, however, expects embeddings that match its own token dimension *E*. To reconcile this mismatch we train a lightweight MLP (or a single linear layer) that transforms the vision features from *D* to *E*.

The steps are:
1. **Vision encoding**: An image is split into patches and encoded, producing *N* vectors.
2. **Feature projection**: A two-layer MLP (with optional non-linearity) or a single linear matrix projects each *D-dimensional* vector to *E* dimensions.
3. **Language model feeding**: The projected tokens are concatenated with the textual prompt and fed to a VLM such as Qwen2-VL-2B-Instruct.

Because the projection operates per-patch, it preserves spatial information and can be fine-tuned on a small amount of domain-specific data (e.g., marine organism images). This enables zero-shot or few-shot inference on high-resolution aerial frames.

## Interactive Notebook
The hands-on implementation can be explored in the notebook:
[VLM_MLP_Projection.ipynb](VLM_MLP_Projection.ipynb)

## Key Topics
- Two-layer MLP projection module bridging vision encoder to language model
- Weight shape calculations and parameter counting
- Direct unauthenticated access to LILA BC Community Fish Detection dataset
- Zero-shot marine organism identification with Qwen2-VL-2B-Instruct and InternVL2-1B
- Batch evaluation and structured CSV export

## References
- LLaVA: Visual Instruction Tuning (NeurIPS 2023)
- InternVL 2.0: Expanding Performance Boundaries of Open-Source Multimodal Models (2024)
- Qwen2-VL: To See the World More Clearly (2024)
- LILA BC Community Fish Detection Dataset: https://lila.science/datasets/community-fish-detection-dataset/

