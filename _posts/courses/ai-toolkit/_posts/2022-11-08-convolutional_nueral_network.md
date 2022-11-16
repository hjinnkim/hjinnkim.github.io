---
title: "[AI toolkit] 12. Convolutional Neural Network"
excerpt: "Basics of CNNs"

categories:
  - AI toolkit
tags:
  - [Deep Learning, CNN]

permalink: /courses/ai-toolkit/012/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-11-14
last_modified_at: 2022-11-14
order: 13
published: true
use_math: true

---
# Lecture 12

## 0. Table of Contents

1. [MNIST Classification with MLPs](#1-mnist-classification-with-mlps)
2. [Convolutional Neural Networks](#2-convolutional-neural-networks)
3. [Stacking Layers](#3-stacking-layers)

## 1. MNIST Classification with MLPs

<p align="center">
  <img src="{{site.gdrive_url_prefix}}1cme5S_V2z4W6rYtDYo7PEg8va_J9JaJg" title="lec12-p3 image" style="float: center; width:100%">
</p>

## 2. Convolutional Neural Networks

### 2.1 Convolution Layers

- Number of parameters: 3x224x224x256+256x128+128x10 = 38569216
  - 3x224x224 resolution is very low. But, # of parameters is very high
  - Need More efficient way for processing images
- Images can be similar even if scale is changed, or transition happens.
    <p align="center">
      <img src="{{site.gdrive_url_prefix}}1MMr7gzn6Q3YIzrZjyHBGHbOPnyIZ-eo6" title="lec12-p7 image" style="float: center; width:30%">
    </p>
  - Characteristics of images can be considered to be hierarchical
    <p align="center">
      <img src="{{site.gdrive_url_prefix}}1PyVejbWX-oAZbCtGb-ifQras72zYdZcV" title="lec12-p7 image" style="float: center; width:80%">
    </p>
- For an image with size WxHx3:
    <p align="center">
      <img src="{{site.gdrive_url_prefix}}1YtAUaM_pfW_4jF5tHVMi_A2eA-JssIHL" title="lec12-p14 image" style="float: center; width:80%">
    </p>
  - Filter size : 3x3x3 : (WxHx3)
  - Filters are shared when processing different area of an image
  - Look at the local parts first, not the whole part of the image
- Multiple filters gives multiple channel output
  - K filters gives MxNxK responses(WxHxC)
    <p align="center">
      <img src="{{site.gdrive_url_prefix}}1E10olBIUNaCdIyzWSvodZ1YUGbQcDaoS" title="lec12-p16 image" style="float: center; width:100%">
    </p>

### 2.2 Pooling Layers

- Subsampling the response with pooling layers
    <p align="center">
      <img src="{{site.gdrive_url_prefix}}1IHS_p10NGezGUgR58KKtaQvYAPwaTJg1" title="lec12-p24 image" style="float: center; width:100%">
    </p>
- Reprentative pooling layers:
  - Max pooling layer
  - Average pooling layer

## 3. Stacking Layers

- Convolution layer $\rightarrow$ Activation func $\rightarrow$ Subsampling by pooling $\rightarrow$ ~~~
    <p align="center">
      <img src="{{site.gdrive_url_prefix}}1Ixu5X3YMFx4v-JF5grBNQfMllPLgLdQa" title="lec12-p16 image" style="float: center; width:100%">
    </p>

## 4. Convolution Layer Output Size

- Kernel size, stride, dilation, padding

    <p align="center">
      <img src="{{site.gdrive_url_prefix}}1N_YcNwBOltdHXlDl3uzsqRRTHQCUlMhz" title="lec12-p30 image" style="float: center; width:100%">
    </p>
