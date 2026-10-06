---
title: VLM QFormer
---

*Placeholder content - original file not found in upstream repository.*
# 4.4 VLM: Querying Transformer (Q-Former) Feature Compression
{: #lesson-vlm-qformer}

## Overview
A Querying Transformer (Q-Former) uses learnable query tokens and cross-attention to compress variable-length visual representations into a fixed-size token budget before feeding them to a language model.

## Lesson Narrative
In this lesson we demonstrate how to insert a lightweight Q-Former module between a vision encoder and a large language model. The vision encoder produces a sequence of image patch embeddings, which can vary in length depending on the image resolution. Directly passing these embeddings to a language model would overwhelm the token limit. The Q-Former solves this by learning a small set of query vectors that attend to the full set of image tokens and distill their information into a fixed number of output tokens.

The procedure consists of three steps:
1. **Vision encoding**: The image is split into patches and encoded by a pretrained vision backbone (such as a small CLIP model). This yields a tensor of shape *(N, D)* where *N* is the number of patches.
2. **Q-Former projection**: A set of *K* learnable query vectors (typically 32) attend to the *N* patch embeddings via cross-attention. The output is a compact *K*-token representation.
3. **Language model integration**: The *K* tokens are concatenated with the textual prompt and processed by a VLM such as Qwen2-VL-2B-Instruct.

By compressing the visual information, the Q-Former enables zero-shot classification and detection on high-resolution inputs while staying within the language model's context window.

## Interactive Notebook
The hands-on implementation can be explored in the notebook:
[VLM_QFormer.ipynb](VLM_QFormer.ipynb)

## Key Topics
- Cross-attention query mechanism
- Fixed-size token budget for variable-size images
- Integration with lightweight Chinese VLMs
- Direct unauthenticated access to the LILA BC MIT Sea Grant River Herring dataset
- Structured pandas reporting of compressed features

## References
- BLIP-2: Bootstrapping Language-Image Pre-training (ICML 2023)
- InstructBLIP: Towards General-purpose Vision-Language Models with Instruction Tuning (NeurIPS 2023)
- LILA BC MIT Sea Grant River Herring Dataset: https://lila.science/datasets/mit-sea-grant-river-herring/

