---
title: Structured Knowledge Distillation for Semantic Segmentation
date: 2019-05-03 19:08:58
categories:
- CV
tags:
- Semantic Segmention
- Knowledge Distillation
---

> - CVPR 2019
> - arXiv https://arxiv.org/abs/1903.04197

<!-- more -->

## Motivation
- The need of neural netoworks with small model size, light computation cost and high segmentation accuracy for applications on mobile devices.
- Deep neural networks have achieved significant improvement in segmentation accuracy.
- Knowledge distillation has been verified valid in classification tasks.

## Contributions
- Knowledge distillation for accurate compact semantic segmentation network training.
- Two structured knowledge distillation strategies for semantic segmentation, pair-wise distillation and holistic distillation.
- Experimental validation on three datasets with different netowrk configurations.

## Methods
T for teacher network (PSPNet with ResNet101), S for Student network (ResNet18, MobileNetV2Plus, ESPNet-C, and ESPNet).
- **Overall architecture**
  Consists of three parts: (a) Pair-wise distillation; (b) Pixel-wise distillation; (c) Holistic distillation.
  <img src="Fig1.PNG" width="550"  align=center />
- **Structured knowledge distillation**
  - Pixel-wise distillation
    Treat segmentation task as a collection of separate pixel classification problem. Direct align the class probability of each pixel produced by S with that produced by T.
    Pixel-wise distillation loss:
    <img src="Fig2.PNG" width="250"  align=center />
    q<sub>i</sub><sup>s</sup> is the class probabilities from S. q<sub>i</sub><sup>t</sup> is the class probabilities from T. KL() is the Kullback-Leibler divergence. R refers to all the pixels.
  - Pair-wise distillation
    Pair-wise distillation is inspired by the pair-wise Markov random field framework. Instead of the pixel probabilities, pair-wise similarities among pixels are transfered.
    Similarity between two pixels is defined as:
    <img src="Fig3.PNG" width="250"  align=center />
    a<sub>ij</sub><sup>s</sup> refers to the similarity between the i<sup>th</sup> pixel and j<sup>th</sup> pixel produced by S. a<sub>ij</sub><sup>t</sup> refers to the similarity between the i<sup>th</sup> pixel and j<sup>th</sup> pixel produced by T. 
    In implementation, the similarity between two pixels is simplified as:
    <img src="Fig4.PNG" width="200"  align=center />
    f<sub>i</sub> and f<sub>j</sub> are two feature maps.
  - Holistic distillation
    Conditional generative adversarial learning is employed. S is treated as the generator conditioned on the input image I. The segmentation map Q<sup>s</sup> is a fake sample. Q<sup>t</sup> is regarded as a real sample. Q<sup>s</sup> needs to be as similar as possible to Q<sup>t</sup>. Wasserstein distance is used to evaluate the difference between Q<sup>s</sup> and Q<sup>t</sup>.
    <img src="Fig5.PNG" width="250"  align=center />
    E[] is the expectation operator. D() is an embedding network including five convolutions with two self-attention modules inserted between the final three layers. Q<sup>s</sup> and I are concatenated and input into D().
- **Optimization and training**
  Overall loss function:
  <img src="Fig6.PNG" width="250"  align=center />
  Optimization in two steps:
  - Train the discriminator by minimizing l<sub>ho</sub>(S,D)
  - Train the segmentation network S by minimizing 
    <img src="Fig7.PNG" width="250"  align=center /> 
    (<img src="Fig8.PNG" width="180"  align=center />)

## Results
- **Effectiveness of the three distillation strategies**
  <img src="Fig9.PNG" width="450"  align=center />
  <img src="Fig10.PNG" width="400"  align=center />
- **Cityscapes**
  <img src="Fig11.PNG" width="450"  align=center />
  <img src="Fig12.PNG" width="900"  align=center />
- **CamVid**
  <img src="Fig13.PNG" width="450"  align=center />
  <img src="Fig14.PNG" width="460"  align=center />
- **ADE20K**
  <img src="Fig15.PNG" width="460"  align=center />

## Conclusion
- A new strategy to incorporate GAN into segmentation.Directly enforce the alignment between the segmentation map and the ground truth may limite the success of the discriminator of GAN as there is mismatch between the generator's continuous output and the discrete true labels. Here, the segmentation map is compared with the continuous output of the teacher network.
- The pixel-wise and holistic distillations were applied on the final score maps and the pair-wise distillation was applied on the feature maps of the last layer. For the comparison, it seems that attention transfer was only applied on the score maps. The full capacity of attention transfer may not be utilized.