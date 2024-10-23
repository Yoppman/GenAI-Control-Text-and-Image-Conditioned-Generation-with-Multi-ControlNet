# GenAI-Control: Text and Image Conditioned Generation with Multi-ControlNet

## Overview

This project, **Multi-ControlNet**, extends the functionality of the ControlNet model by incorporating **separate prompts** for more complex and refined control over image generation tasks. Our aim is to overcome the limitations of standard ControlNet by adding textual conditions that guide image generation based on specific characters or styles. 

Specifically, it is based on **Stable Diffusion** architecture and explores various methods to merge text-based control with image conditions to improve the model's ability to generate highly specific outputs. We experimented with three methods to accomplish this: **direct ControlNet training with prompts**, **DreamBooth integration**, and **LoRA fine-tuning**.

## Table of Contents

- [Motivation](#motivation)
- [Introduction](#introduction)
- [Methods](#methods)
- [Experiments and Results](#experiments-and-results)
- [Conclusion](#conclusion)

---

## Motivation

ControlNet is a powerful extension of the Stable Diffusion model that allows for conditioning on various image features, such as **openpose**, **canny edges**, or **depth maps**. However, the current limitation is that ControlNet cannot bind a **specific text condition** onto the image condition, making it impossible to define detailed textual prompts like a specific character or action. This project addresses that limitation by integrating text prompts into ControlNet.

---

## Introduction

**ControlNet** is an architecture that controls the Stable Diffusion model by adding an additional network copy, trained with specific image conditions. However, the current method lacks support for **text prompts**, preventing users from specifying distinct character features in the generated images.

In this project, we focus on training ControlNet with an additional text condition to generate images conditioned on both text (specific character or action) and image features (pose, depth, etc.). 

---

## Methods

We experimented with three different approaches to integrate text prompts into ControlNet:

1. **Direct Training with Text Prompts**:
    - This method incorporates text prompts directly during ControlNet's training phase. While promising, this approach suffered from **overfitting** due to the limited dataset and the large parameter size of the Stable Diffusion model.

2. **DreamBooth Integration**:
    - DreamBooth is designed to fine-tune pre-trained Stable Diffusion models with new text prompts. By adding a **Prior-Preservation Loss**, DreamBooth minimizes overfitting while preserving the model's original ability to generate accurate outputs. This method produced the best results in our experiments, although some generated images still lacked distinct character features.

3. **LoRA Fine-Tuning**:
    - We used **Low-Rank Adaptation (LoRA)** to fine-tune the ControlNet model. LoRA reduces the overfitting problem by optimizing specific influential aspects during fine-tuning. However, our results showed that LoRA could not effectively resolve the overfitting issue, and the ControlNet model lost its ability to properly manage both text and image conditions.

---

## Experiments and Results

### Dataset:
- **Old Dataset**: Screenshots from anime and Pinterest images.
- **New Dataset**: MPII Human Pose dataset combined with Openpose for pose generation.

### Experiment 1: Direct Training on ControlNet
- The direct training approach generated images but was prone to **overfitting**, producing nearly identical characters without the distinct control based on the text prompts.

### Experiment 2: DreamBooth on ControlNet
- Using DreamBooth provided the best balance between **text-prompt specificity** and image generation, maintaining ControlNet's ability to use image conditions while integrating the character-specific prompt. However, some outputs lacked clarity in character features.

### Experiment 3: LoRA Fine-Tuning
- LoRA failed to optimize the ControlNet's performance with text prompts, leading to degraded image quality and losing control over image conditions.

---

## Conclusion

- Direct training with ControlNet resulted in overfitting and limited control over specific text prompts.
- DreamBooth was the most successful approach, allowing for **separate text prompts** and image conditions, but still exhibited some limitations in the clarity of character-specific features.
- LoRA fine-tuning did not perform as expected, leading to a loss of image control features.

Overall, integrating textual conditions into ControlNet presents unique challenges, particularly with respect to maintaining fine control over both text and image conditions simultaneously. While DreamBooth showed promise, further experimentation and dataset expansion could improve the ability to generate character-specific outputs.

---

## Future Work

- Expanding the dataset to include more diverse character and pose conditions.
- Experimenting with other fine-tuning methods and architectures to reduce overfitting.
- Enhancing DreamBooth integration to improve the distinctness of character features based on text prompts.

## References

- ControlNet: Lvmin Zhang, Maneesh Agrawala. Adding Conditional Control to Text-to-Image Diffusion Models. arXiv:2302.05543
- DreamBooth: Nataniel Ruiz, Yuanzhen Li, Varun Jampani, et al. Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation. arXiv:2208.12242
- LoRA: Edward J. Hu, et al. Low-Rank Adaptation of Large Language Models. arXiv:2106.09685
