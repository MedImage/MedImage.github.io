---
title: Cross modal distillation for supervision transfer
date: 2019-03-07 23:15:43
categories:
- CV
tags:
- Knowledge Distillation
- Attention
---

> Published in 2016 CVPR
>
> - **paper:**  [Cross modal distillation for supervision transfer](https://arxiv.org/abs/1507.00448)
> - **code:** [s-gupta/fast-rcnn](https://github.com/s-gupta/fast-rcnn/tree/distillation)

<!-- more -->

## Motivation

- Most transfer learning happens within the same image modality.
- Modality like RGB images have millions of  labeled data, whereas modality like depth images contain large amounts of unlabeled data but much fewer labeled images, which could not support direct finetuning.

## Novelty

- Supervision transfer through high quality image representations from labeled images of one modality to completely unlabeled images of a new modality.

## Methods

- Build image pairs that contain one image from the labeled dataset and the other from the unlabeled dataset.
- When training the network, build constraints that the learned representations for images of unlabeled dataset matches the learned representations for the corresponding images of labeled dataset for chosen and fixed layers (minimize L2 loss).
  <img src="Fig1.PNG" width="450" hegiht="350" align=center />

## Experimental details

- In the article, supervision transfer was only applied to the features of the last convolutional layer pool5.
- When the features from the two networks (one for each modality) have different resolutions, adjust padding to make them the same.
- An adaptation layer was added to the features from the unlabeled dataset (1x1 conv + ReLU) before the calculation of L2 loss.
- A scaling layer was included to make the average norm of the respective features comparable.

## Results

- NYUD2 dataset (object detection)
  - Supervision transfer is working (compare 1C to 1A and 1B)
    <img src="Fig2.PNG" width="800" hegiht="600" align=center />
  - Supervision transfer cheme transfers good representations (compare 2B to 2A & compare 2C to 2B)
  - The learned representation from the depth images is complementary to that learned from the RGB images (compare 3C to 3A) (average the results from the two modalities)
  - The intermediate layer representations also carry useful information.
  - The layers to be supervision transfered is important.
    <img src="Fig3.PNG" width="450" hegiht="300" align=center />
  - Feasibility to be applied to zero-shot detection on depth images.
    <img src="Fig4.PNG" width="450" hegiht="300" align=center />
  - Achieved state-of-the-art results on the dataset.
    <img src="Fig5.PNG" width="400" hegiht="250" align=center />
- JHMDB dataset (action detection)
  - Similar improvements were achieved.

## Conclusion

- Supervision transfer was successfuly employed to multi-modal image analysis.
- The supervision content used was the features directly, which could be improved.
- The training procedure could be optimized (train with supervision transfer (from the first layer to the transfer layer) -> fix the trained parameters -> optimize the parameters of the subsequent layers with the specific task or just employ the parameters of the trained network on the labeled dataset).
- Which layer or layers to transfer is important. The shallow layers should not be considered as they are domain specific.