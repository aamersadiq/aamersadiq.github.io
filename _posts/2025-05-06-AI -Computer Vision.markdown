---
title: "AI - Computer Vision"
date: 2025-05-06 16:37:42
categories: [ai]
tags: [ai]
---

Computer Vision, a subfield of Artificial Intelligence, focuses on enabling machines to extract, analyze, and interpret meaningful information from visual data. With the growth of computational power, data availability, and deep learning techniques, Computer Vision has evolved from basic image processing to complex visual understanding tasks.
Lets explore the technical foundations of AI in Computer Vision, including key algorithms, architectures, datasets, and evaluation metrics. I will also examine current challenges, emerging trends, and the future trajectory of this rapidly advancing domain.


<h3>Historical Context</h3>
Computer vision has evolved dramatically over the past few decades. Early systems relied on handcrafted features such as edges and corners getting extracted using algorithms such as SIFT and HOG. These approaches worked well for simple tasks but struggled with real world complexity.
The turning point came in 2012, when AlexNet, a deep Convolutional Neural Network (CNN), won the ImageNet challenge by a wide margin. This victory demonstrated that neural networks could outperform traditional methods in image classification, sparking a revolution in computer vision. Since then, CNNs have become the foundation of nearly every modern vision system.

<h3>Convolutional Neural Networks (CNNs)</h3>
One of the most common machine learning model architectures for computer vision is the Convolutional Neural Network (CNN), a deep learning architecture designed to process visual data. CNNs are particularly effective at tasks like image classification, object detection, and segmentation because they learn to extract meaningful patterns from pixel data.

<h3>How CNNs Works</h3>
CNNs use filters (also known as kernels) to scan across an image and extract numeric feature maps. These maps represent visual patterns such as edges, textures, and shapes. The extracted features are then passed through deeper layers of the network to generate a label prediction essentially answering the question, “What is this an image of?”
For example, in an image classification scenario, you might train a CNN model with images of different kinds of fruit such as apples, bananas, and oranges. The model learns to associate certain visual features with each fruit type. When presented with a new image, the CNN predicts the label: “This is a banana.”

<h4>Training a CNN</h4>
During the training process:
<ul>
    <li>Filter kernels are initially defined using randomly generated weight values.</li>
	<li>The model processes labeled images and makes predictions.</li>
	<li>These predictions are evaluated against known label values.</li>
	<li>The filter weights are adjusted to improve accuracy.</li>
</ul>
Eventually, the trained fruit image classification model uses the filter weights that best extract features that help identify different kinds of fruit. This process allows CNNs to learn which visual characteristics like shape, color, and texture are most useful for distinguishing between classes.

<h4>Core Concepts & Techniques</h4>
CNNs are built on several key principles that make them effective:
<ul>
    <li><span style="font-weight: bold;">Local connectivity: </span>Neurons connect to small regions of the input, preserving spatial relationships.</li>
    <li><span style="font-weight: bold;">Weight sharing: </span>Filters are reused across the image, reducing the number of parameters.</li>
    <li><span style="font-weight: bold;">Activation functions: </span>Non-linear functions like ReLU introduce complexity.</li>
    <li><span style="font-weight: bold;">Pooling: </span>Downsampling reduces dimensionality and improves efficiency.</li>
    <li><span style="font-weight: bold;">Backpropagation: </span>Errors are propagated backward to update weights.</li>
    <li><span style="font-weight: bold;">Regularization: </span>Techniques like dropout and batch normalization prevent overfitting.</li>
</ul>
These concepts enable CNNs to learn complex patterns while remaining computationally efficient

<h4>Key Architectures</h4>
Over time, CNNs have evolved into deeper and more sophisticated architectures:
<table style="width:70%; border-collapse:collapse; font-family:Arial, sans-serif; margin:20px 0;">
  <thead>
    <tr>
      <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Architecture</th>
      <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Key Features</th>
      <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Use Case</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">AlexNet</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">First deep CNN to win ImageNet</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Image classification</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">VGGNet</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Uniform architecture with small filters</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Transfer learning</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">ResNet</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Introduced residual connections</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Very deep networks</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">Inception</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Multi-scale feature extraction</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Efficient computation</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">DenseNet</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Dense connections between layers</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Feature reuse</td>
    </tr>
  </tbody>
</table>

<h3>Datasets & Benchmark</h3>

<h3>Applications Across Domains</h3>

<h3>Challenges & Limitations</h3>

<h3>Future Directions</h3>

