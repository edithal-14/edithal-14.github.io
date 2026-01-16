---
layout: post
title: Dynamic Visual Quality Assessment for Video Games 
subtitle: Extract gameplay visual quality indicators like anti-aliasing and texture quality using EfficientNet to predict overall visual quality by training regressors
thumbnail-img: images/game_vqa/texture_quality.jpg
cover-img: images/game_vqa/texture_quality.jpg
tags: [Visual Quality, Texture Quality, Video Games, Graphics Recommendation, EfficientNet, Feature Extraction, Regression]
---

### Abstract

In this work, we introduce a deep learning solution to judge video quality for gaming by leveraging no-reference datasets of gameplay videos.
Existing no-reference visual quality assessment (VQA) methods perform poorly on computer generated graphics application such as video games.

We develop an EfficientNet-based feature extractor to detect important indicators of gameplay visual quality such as anti-aliasing, texture quality.
This feature extractor was inspired from Google's Universal Video Quality model **[1]**.
Our feature extractor is able to detect game graphics artifacts like shadows, texture in multiple games from racing genre as shown in the diagram below.

We used multiple binary classifier student models trained to predict the presence of artifacts on a per-frame basis, and used their logits as soft scores to train the distilled teacher model.
The feature extractor was obtained by removing the head of the distilled model which can predict the amount of distortions (aliasing, texture blur, shadow blur) on a continuous scale.

An overall neural network quality regressor takes the extracted features as input and weights them using scene semantics obtained from a CLIP model and outputs and overall quality score.
For example, shadow blur is weighted less in scenes with dim lighting, whereas texture quality is weighted more in scenes with high density of information (e.g. a city scene).

We then finetune the neural network regressor on a dataset of videos collected at different graphics settings level and quality presets.
The dataset collected was automated using a game interaction script which simulates keyboard and mouse inputs to leverage the internal game assists to automatically complete races and collect dataset using OBS Studio.
The predicted game quality scores from the regressor show strong correlation with in-built game presets.

Since our model is entirely based on CNNs, it can be well optimized using ONNX framework for deployment in production.
Deploying such models to production will help us understand the visual quality performance of various applications on a broad set of GPUs.

To conclude, our work provides robust visual quality scores for games in the racing genre.
A limitation of our work is the lack of human-labelled Mean Opinion Score (MOS) data for training visual quality score regressor.
Hence, we are limited to using in-built preset qualities in games to train our model which makes our model less generalizable to unseen games and more prone to over-fitting.

### Images

![Example of distortions in video games]({{ '/images/game_vqa/distortion_dataset.png' | relative_url }})

The five quality indicators in the distortion dataset.
**Aliasing** appears as jagged or stair-stepped outlines.
**Texture blur** indicates low-resolution textures.
**Geometry density** reflects low levels of detail in 3D objects.
**Ambient occlusion** is evident in contact shadows, the absence of which makes objects appear to float.
Shadow quality refers to missing shadows or shadows rendered at low resolution

![Overview of Game VQA model]({{ '/images/game_vqa/gdfe_model_arch.png' | relative_url }})

An overview of the Game VQA model architecture featuring the Game Distortion Feature Extractor obtained using distillation. The CLIP model outputs used for weighting features and the final quality regression output is also shown.

![Game VQA results]({{ '/images/game_vqa/game_vqa_results.jpg' | relative_url }})

Distribution of aliasing and texture artifact detection for videos recorded at various graphics quality presets.
Forza Horizon 4 and Forza Horizon 5 test videos were used to plot this chart while the model was trained on Forza Horizon 4 train videos.
Distortion free probability represents graphical proximity to highest quality videos.
The red cross represents average across all images in the test videos per graphics quality preset.

### Acknowledgements

Thumbnail photo for the blog post was taken from **[2]**

All credits for this work goes to the authors of this paper **[3]** 

### Link to research report

[Click here](https://arxiv.org/abs/2505.11724) to see the research report uploaded to arxiv

### References

**[1]** Wang, Yilin and Ke, Junjie and Talebi, Hossein and Yim, Joong Gon and Birkbeck, Neil and Adsumilli, Balu and Milanfar, Peyman and Yang, Feng. Rich Features for Perceptual Quality Assessment of UGC Videos. Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR). June 2021. [Universal Video Quality blog post](https://research.google/blog/uvq-measuring-youtubes-perceptual-video-quality/)

**[2]** https://gamersnexus.net/game-bench/1949-witcher-3-texture-quality-comparison-vram-and-fps

**[3]** https://arxiv.org/abs/2505.11724