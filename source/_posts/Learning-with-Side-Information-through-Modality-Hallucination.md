---
title: Learning with Side Information through Modality Hallucination
date: 2019-03-08 20:42:34
categories:
- CV
tags:
- multimodal
---

> Published in 2016 CVPR
>
> - **paper:**  [Learning with Side Information through Modality Hallucination](http://openaccess.thecvf.com/content_cvpr_2016/html/Hoffman_Learning_With_Side_CVPR_2016_paper.html)

<!-- more -->

## Motivation

- Depth image capturing devices are much less prevalent than RGB image capturing devices.
- Although models utilize both modalities can produce better recognition results, they need to perform well when only have RGB inputs.

## Novelty

- A novel convolutional network model, which is trained with RGB-D images, that can perform better with only RGB inputs when compared to models trained on RGB images.
- A modality hallucination module  which regresses the hallucination features to the auxiliary modality features (here depth image features).
- During testing, although only RGB inputs are given, the model can generate both RGB features and hallucination features (expected to mimic depth features).

## Methods

- Architecture
  <img src="Fig1.PNG" width="800" hegiht="650" >
- Training procedure
  - Independently train the RGB and depth networks (Fast R-CNN).

  - Initialize the hallucination network with the depth network parameters.

  - Finetune the whole network in Figure 1. (alpha=0.5, beta=1.0, gamma is determined by running several iterations so that the hallucination loss is roughly 10 times larger than the other losses) (freeze the depth network lower than the hallucination loss (pool5)).
    <img src="Fig2.PNG" width="400" hegiht="250" >

    <img src="Fig3.png" width="450" hegiht="300" >
- Gradient clipping to decrease the influence from the outliers (If the total gradient exceeds threshold T (10), all gradients are scaled by T/ total norm (L2 norm)).
- HHA encoding of the depth images.

## Experiments & Results

- NYUD2 detection
  - Overall results (better than RGB only and ensemble (two networks)).
    <img src="Fig4.PNG" width="900" hegiht="600" >
  - The initialization of the hallucination network is important (initialize with the parameters of the depth network is the best)..
    <img src="Fig5.PNG" width="900" hegiht="600" >
  - The activation layer to hallucinate is important (pool5 is the best)..
    <img src="Fig6.PNG" width="900" hegiht="600" >
  - The proposed hallucination approach is also useful for other datasets (Pascal dataset VOC 2007).
  - The features from the hallucination network is closer to those from the depth network than from the RGB network.

## Conclusion

- The learning of hallucination features is similar to image generation approach. However, the authors state that their method works better than direct hallucination of depth images.
- Will the method perform better than two modality networks in testing? Never mentioned.
- The method could be extended to include other side information into networks.

