---
title: Fusion Network for RGB-D Salient Object Detection
date: 2019-03-14 14:08:23
categories:
- CV
tags:
- multimodal
---

## 1. Progressively Complementarity-aware Fusion Network for RGB-D Salient Object Detection

> Published in 2018 CVPR
>
> **paper:** [Progressively Complementarity-aware Fusion Network for RGB-D Salient Object Detection](http://openaccess.thecvf.com/content_cvpr_2018/html/Chen_Progressively_Complementarity-Aware_Fusion_CVPR_2018_paper.html)

<!-- more -->

- Main novelty:
  - Design a complementarity-aware fusion (CA-Fuse) module, which introduces cross-modal residual functions and complementarity-aware supervisions (side loss)
  - Add level-wise supervision from deep to shallow densely
- Overall architecture
  <img src="Fig1.PNG" width="750"  align=center />
- CA-Fuse module
  <img src="Fig2.PNG" width="750"  align=center />
- Details: Add a large kernel convolution (Conv6, 13X13) and include five set of side loss functions (weights all set to 1) plus a loss to encourage informative combination of all side outputs.
  <img src="Fig3.png" width="300"  align=center />
- Results
  <img src="Fig4.PNG" width="750"  align=center />

  

## 2. Attention-aware Cross-modal Cross-level Fusion Network for RGB-D Salient Object Detection

> Published in 2018 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)
>
> **paper:** [Attention-aware Cross-modal Cross-level Fusion Network for RGB-D Salient Object Detection](https://scholars.cityu.edu.hk/en/publications/attentionaware-crossmodal-crosslevel-fusion-network-for-rgbd-salient-object-detection(38d5cc13-4d46-4086-984b-e9081a09081e).html)

- Main novelty: Proposed an attention-aware cross-modal cross-level fusion (ACCF) module to fuse different level RGB and depth features. The ACCF module is similar to the SE block.
- Details: Add a large kernel convolution (Conv6, 13X13) and include five  loss functions  (weights all set to 1).
  <img src="Fig5.PNG" width="750"  align=center />
- Results (worse than the CVPR results)
  <img src="Fig6.PNG" width="450"  align=center />



## 3. Multi-Modal Fusion Network with Multi-Scale Multi-Path and Cross-Modal Interactions for RGB-D Salient Object Detection

> Published in 2019 Pattern Recognition
>
> **paper:** [Multi-Modal Fusion Network with Multi-Scale Multi-Path and Cross-Modal Interactions for RGB-D Salient Object Detection](https://www.sciencedirect.com/science/article/pii/S0031320318303054)

- Main novelty:
  - Propose a global understanding branch (pooling) and a local capturing branch (realized through dilated convolution).
  - Multi-layer fusion by element-wise summation.
- Architecture
  <img src="Fig7.PNG" width="750"  align=center />
- Details
  - Train the R_SalNet first by VGG initialization. Then train the D_SalNet with R_SalNet initialization. Finally, fintune the whole network with paired inputs.
  - FC layer outputs (3136 vector) is warped to the saliency map of 56 x 56.
- Results
  - Achieve better results when compared to state-of-the-arts methods (seems even worse then the IROS paper).
    <img src="Fig8.PNG" width="750"  align=center />
  - The fusion direction is important (depth to RGB works best). The authors stated that the 'MP+CI-Bi' method introduced too much parameters  and the bi-directional connections may destroy the fragile architecture. Since the fusion was achieved by summation, no additional parameters should be introduced. The first reason seems unreasonable.
    <img src="Fig9.PNG" width="450"  align=center />