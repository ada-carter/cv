---
title: VLM Native Multimodal
---

*Placeholder content - original file not found in upstream repository.*
# 4.5 VLM: Native Multimodal Integration
{: #lesson-vlm-native-multimodal}

## Overview
Native multimodal integration processes image and text inputs jointly within a single transformer, allowing the model to reason across modalities without separate encoders.

## Lesson Narrative
In this lesson we explore a Vision-Language Model that directly accepts both visual and textual tokens in a unified architecture. Unlike pipelines that first encode images then merge embeddings, a native multimodal model (e.g., InternVL2-2B) embeds images and text into a shared space and applies cross-attention throughout the transformer layers.

The workflow consists of three phases:
1. **Unified tokenization**: Images are split into patches and projected to token embeddings; text is tokenized with a language model tokenizer. Both token streams are concatenated.
2. **Joint transformer processing**: The combined token sequence passes through standard transformer blocks where self-attention mixes visual and textual information.
3. **Multimodal output**: The model can generate text conditioned on images (captioning) or produce image-grounded responses to textual queries (visual question answering).

By keeping the modalities in a single stream, the model can capture fine-grained interactions such as spatial references ("the bird on the left") and visual grounding of textual descriptions.

## Interactive Notebook
Explore the implementation in the notebook:
[VLM_Native_Multimodal.ipynb](VLM_Native_Multimodal.ipynb)

## Key Topics
- Joint token embedding of image patches and text
- Cross-modal self-attention across transformer layers
- Zero-shot visual question answering on marine datasets
- Direct unauthenticated access to LILA BC Native Multimodal datasets (e.g., seabird images with accompanying species labels)
- Generating multimodal outputs and parsing them into structured results

## References
- InternVL 2.0: Expanding Performance Boundaries of Open-Source Multimodal Models (2024)
- Qwen2-VL: To See the World More Clearly (2024)
- LILA BC MIT Sea Grant River Herring Dataset: https://lila.science/datasets/mit-sea-grant-river-herring/

