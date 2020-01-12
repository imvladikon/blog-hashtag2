---
layout: post
title:  "Deep learning Java library"
date:   2020-01-12 11:59:16 +0200
categories: 
description: 
tags: java deeplearning
---
As we've heard probably there is a ML toolkit based on Java, - [DL4J](https://deeplearning4j.org/) from Eclipse. But now we have another [one library for ML](https://djl.ai/) on Java from AWS team. And it's good. 

<img src="/assets/img/ml_mashups.jpg" alt="ml_mashups" width="250"/>

<!--break-->
I can watch endlessly dancing graphs. But let's see, what we have. 

<img src="/assets/img/dl4j_sites.png" alt="dl4j_sites" width="250"/>

Amazon released Deep Java Library, an open-source library for training, testing, deploying, and making predictions with deep-learning models.

<img src="/assets/img/1deep-java-library-1578484330456.jpg" width="250">

There are jupyter's notebooks [here](https://github.com/awslabs/djl/blob/master/jupyter/README.md). 
[Maven](https://mvnrepository.com/artifact/ai.djl):
```
<!-- https://mvnrepository.com/artifact/ai.djl.mxnet/mxnet-engine -->
<dependency>
    <groupId>ai.djl.mxnet</groupId>
    <artifactId>mxnet-engine</artifactId>
    <version>0.2.0</version>
</dependency>
```
Deep Java Library supports training on multiple GPUs. 
```java
TrainingConfig config =  new DefaultTrainingConfig(initializer, loss)
    .setOptimizer(optimizer)
    .setBatchSize(batchSize)
    .setDevices(Device.getDevices(1))
    .addEvaluator(accuracy);
```
We can use setDevices and pass an array of devices we want the model to be trained on. For example, new Device[]{Device.gpu(0), Device.gpu(1)} for training on GPU0 and GPU1. 