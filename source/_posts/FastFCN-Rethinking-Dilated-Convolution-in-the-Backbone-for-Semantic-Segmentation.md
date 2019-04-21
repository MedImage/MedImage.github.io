---
title: FastFCN Rethinking Dilated Convolution in the Backbone for Semantic Segmentation
date: 2019-04-22 00:17:09
categories:
- CV
tags:
- Semantic Segmention
---

> - arXiv https://arxiv.org/abs/1903.11816
> - Code https://github.com/wuhuikai/FastFCN

<!-- more -->

## Motivation

- Dilated convolutions are effective but bring heavy computation complexity and memory footprint.

## Contributions

- Propose a computationally efficient joint upsampling module (JPU) to replace diated convolutions.
- Reduce the computation time and memory footprint of the whole segmentation network by a factor of more than 3.
- Achieve the new state-of-the-art performance in Pascal Context dataset and ADE20K dataset.

## Methods

- Joint upsampling
  <img src="Fig1.png" width="450"  align=center />
  yh - high-resolution target image, yl - low-resolution target image, xh - high-resolution guidance image, hl - low-resolution guidance image

- JPU module

  - Problem formulation
    <img src="Fig4.png" width="750"  align=center />
    <img src="Fig2.png" width="450"  align=center />
    <img src="Fig3.png" width="450"  align=center />
    S - split, M - merge, Cr - regular convolution, Cd - dilated convolution, Cs - stride convolution, adjacent S and M operations can be canceled out, ys - output feature map from normal FCN (32x), yd - output feature map from DilatedFCN (8x).

  <img src="Fig5.png" width="400"  align=center />
  y is an approximation of yd. Approximating yd using ys is the same as the joint upsampling problem.

  - Problem solving
    <img src="Fig6.png" width="500"  align=center />
    <img src="Fig7.png" width="700"  align=center />

## Results

- Pascal Context dataset
  <img src="Fig8.png" width="350"  align=center />

   <img src="Fig9.png" width="350"  align=center /> 

  <img src="Fig10.png" width="350"  align=center />

- ADE20K dataset
  <img src="Fig11.png" width="350"  align=center /> 

- <img src="Fig12.png" width="350"  align=center />

## Conclusion

- The JPU module is effective on improving the segmentation results and reducing the computation complexity.
- The analysis regarding formulating the problem of approximating feature output of DilatedFCN based on feature output of normal FCN is interesting but solving the problem using the proposed JPU module is not demonstrated very well.
- The JPU module is like a simple version of ASPP. Taking the three level feature outputs and feed them  to ASPP may generate better results.
- Depthwise separable convolution was utilized and need a deep investigation.