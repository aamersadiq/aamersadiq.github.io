---
title: "AI - Computer Vision"
date: 2025-05-06 16:37:42
categories: [ai, computer-vision, cnns, image-models, ocr]
categories: [ai, computer-vision, cnns, image-models, ocr]
---

Computer Vision, a subfield of Artificial Intelligence, focuses on enabling machines to extract, analyze, and interpret meaningful information from visual data. With the growth of computational power, data availability, and deep learning techniques, Computer Vision has evolved from basic image processing to complex visual understanding tasks.
Lets explore the technical foundations of AI in Computer Vision, including <strong>key algorithms, architectures, datasets, evaluation metrics and common vision tasks</strong>. 


<h3>Historical Context</h3>
Computer vision has evolved dramatically over the past few decades. Early systems relied on handcrafted features such as edges and corners getting extracted using algorithms such as SIFT and HOG. These approaches worked well for simple tasks but struggled with real world complexity.
The turning point came in 2012, when AlexNet, a deep <strong>Convolutional Neural Network (CNN)</strong>, won the ImageNet challenge by a wide margin. This victory demonstrated that neural networks could outperform traditional methods in image classification, sparking a revolution in computer vision. Since then, CNNs have become the foundation of nearly every modern vision system.

<h3>Convolutional Neural Networks (CNNs)</h3>
One of the most common machine learning model architectures for computer vision is the Convolutional Neural Network (CNN), a deep learning architecture designed to process visual data. CNNs are particularly effective at tasks like <strong>image classification, object detection, and segmentation</strong> because they learn to extract meaningful patterns from pixel data.

<h4>How CNNs Works</h4>
CNNs use <strong>filters (also known as kernels)</strong> to scan across an image and extract numeric feature maps. These maps represent visual patterns such as edges, textures, and shapes. The extracted features are then passed through deeper layers of the network to generate a label prediction essentially answering the question, “What is this an image of?”
For example, in an image classification scenario, you might train a CNN model with images of different kinds of fruit such as apples, bananas, and oranges. The model learns to associate certain visual features with each fruit type. When presented with a new image, the CNN predicts the label: “This is a banana.”

<h4>Training a CNN</h4>
During the training process:
<ul>
    <li><strong>Filter kernels</strong> are initially defined using randomly generated weight values.</li>
	<li>The model processes <strong>labeled images and makes predictions</strong>.</li>
	<li>These predictions are evaluated against <strong>known label values</strong>.</li>
	<li>The <strong>filter weights</strong> are adjusted to improve accuracy.</li>
</ul>
Eventually, the trained fruit image classification model uses the filter weights that best extract features that help identify different kinds of fruit. This process allows CNNs to learn which visual characteristics like shape, color, and texture are most useful for distinguishing between classes.

<h4>Core Concepts & Techniques</h4>
CNNs are built on several key principles that make them effective:
<ul>
    <li><strong>Local connectivity: </strong>Neurons connect to small regions of the input, preserving spatial relationships.</li>
    <li><strong>Weight sharing: </strong>Filters are reused across the image, reducing the number of parameters.</li>
    <li><strong>Activation functions: </strong>Non-linear functions like ReLU introduce complexity.</li>
    <li><strong>Pooling: </strong>Downsampling reduces dimensionality and improves efficiency.</li>
    <li><strong>Backpropagation: </strong>Errors are propagated backward to update weights.</li>
    <li><strong>Regularization: </strong>Techniques like dropout and batch normalization prevent overfitting.</li>
</ul>
These concepts enable CNNs to learn complex patterns while remaining computationally efficient.

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
Each architecture builds on the strengths of its predecessors, pushing the boundaries of accuracy and scalability.

<h4>Vision Models: CNNs and Beyond</h4>
Computer vision models come in many architectural flavors. While Convolutional Neural Networks (CNNs) have long been the backbone of visual recognition, newer models like Transformers and Graph Neural Networks are expanding the frontier.
Here is a overview of both CNN based and non CNN based models, organized by task and innovation.
<table style="width:70%; border-collapse:collapse; font-family:Arial, sans-serif; margin:20px 0;">
  <thead>
    <tr>
      <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Model Name</th>
      <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Type</th>
      <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Task Focus</th>
	  <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Key Innovation</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">AlexNet</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">CNN</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Image Classification</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">First deep CNN to win ImageNet</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">VGGNet</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">CNN</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Classification & Transfer</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Uniform architecture with small filters</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">ResNet</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">CNN</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Deep Classification</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Residual connections for deeper networks</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">YOLO</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">CNN</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Real-Time Detection</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Single-pass object detection</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">U-Net</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">CNN</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Image Segmentation</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Symmetric encoder-decoder architecture</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">Vision Transformer (ViT)</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Transformer</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">General Vision Tasks</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Self-attention replaces convolutions</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">Graph Neural Network (GNN)</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Graph-based</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Relational Vision Tasks</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Models data as nodes and edges</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">Capsule Network</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Capsule-based</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Spatial Reasoning</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Preserves part whole relationships
</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">Support Vector Machine (SVM)</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Classical ML</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Classification</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Hyperplane-based decision boundaries</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">Random Forest</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Classical ML</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Classification & Regression</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Ensemble of decision trees</td>
    </tr>
  </tbody>
</table>

<h4>Datasets & Benchmark</h4>
To train and evaluate computer vision models effectively, researchers rely on datasets and benchmarks, two foundational tools that shape the development of intelligent visual systems.

Datasets are curated collections of labeled images (or other visual data) used to teach models how to recognize patterns.
<ul>
    <li>They serve as the training material for machine learning algorithms.</li>
	<li>Labels indicate what each image contains, such as objects, categories, or pixel-level annotations.</li>
	<li>A diverse and well-labeled dataset helps models generalize to real world scenarios.</li>
</ul>
Think of a dataset as the curriculum for a vision model, it is how the model learns.

Benchmarks are standardized tests that evaluate and compare model performance.
<ul>
    <li>They use fixed datasets and scoring metrics (e.g. accuracy, precision, recall).</li>
	<li>Researchers submit models to benchmark challenges to see how well they perform.</li>
	<li>Benchmarks identify state of the art models and track progress across the field.</li>
</ul>
Benchmarks are like the final exam, everyone takes the same test, and the scores reveal who’s best.

Popular Datasets & Benchmarks:
<table style="width:70%; border-collapse:collapse; font-family:Arial, sans-serif; margin:20px 0;">
  <thead>
    <tr>
      <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Name</th>
      <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Type of Task</th>
      <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Description</th>
	  <th style="background-color:#2c3e50; color:#ffffff; font-weight:bold; border:1px solid #ddd; padding:12px 16px; text-align:left;">Benchmark Role</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">ImageNet</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Classification</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">14M+ labeled images across 20K categories</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Image classification</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">COCO</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Detection & Segmentation</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Rich annotations for objects in context</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Object detection, segmentation</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">Pascal VOC</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Detection & Classification</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Early benchmark for object recognition</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Model comparison</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">Cityscapes</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Semantic Segmentation</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Urban scenes with pixel-level labels</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Autonomous driving</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">LUNA16</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Medical Imaging</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">CT scans for lung nodule detection</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Healthcare AI</td>
    </tr>
	<tr style="background-color:#ffffff;">
      <td style="border:1px solid #ddd; padding:12px 16px;">OpenImages</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Multi label Classification</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">Large scale dataset with bounding boxes</td>
      <td style="border:1px solid #ddd; padding:12px 16px;">General purpose vision</td>
    </tr>
  </tbody>
</table>
These datasets and benchmarks are the backbone of modern computer vision research. They ensure fair comparisons, drive innovation, and help developers choose the right models for their applications.

<h3>Common Vision Tasks</h3>
<p>Computer vision covers a range of core tasks that allow machines to understand and interpret visual information. Below are four of the most widely used tasks, each with distinct goals, workflows, and model choices.</p>

<h4>Image Classification</h4>
<p>
Assigns a single label to an entire image, answering the question: <em>"What is this image of?"</em> 
<br><strong>Typical Use Cases:</strong> Product categorisation, species identification, defect detection in manufacturing.<br>
<strong>Approaches:</strong> 
<ul>
  <li><strong>Small dataset:</strong> Transfer learning from pretrained CNNs like ResNet‑50 or MobileNet, with strong augmentation.</li>
  <li><strong>Large dataset:</strong> Train deep architectures (EfficientNet, Vision Transformers) from scratch for maximum accuracy.</li>
</ul>
</p>

<h4>Object Detection</h4>
<p>
Identifies and localises multiple objects in an image by returning bounding boxes with class labels.
<br><strong>Typical Use Cases:</strong> Autonomous driving, security surveillance, retail analytics.<br>
<strong>Approaches:</strong> 
<ul>
  <li><strong>Small dataset:</strong> Fine‑tune pretrained detectors such as YOLOv5 or Faster R‑CNN, combined with data augmentation.</li>
  <li><strong>Large dataset:</strong> Custom‑train models like EfficientDet or Cascade R‑CNN with multi‑scale training.</li>
</ul>
</p>

<h4>Image Segmentation</h4>
<p>
Classifies each pixel in the image, producing a segmentation map that outlines objects or regions.
<br><strong>Typical Use Cases:</strong> Medical imaging (tumour segmentation), autonomous navigation (lane marking detection), satellite imagery analysis.<br>
<strong>Approaches:</strong> 
<ul>
  <li><strong>Real‑time constraint:</strong> Lightweight networks like BiSeNet or Fast‑SCNN for speed‑critical applications.</li>
  <li><strong>No real‑time constraint:</strong> Accuracy‑first architectures like DeepLabv3+ or Mask R‑CNN.</li>
</ul>
</p>

<h4>Optical Character Recognition (OCR)</h4>
<p>
Extracts text from images or video frames, converting it into machine‑readable form. Modern OCR often uses CNNs for feature extraction, coupled with RNNs or Transformers for sequence decoding.
<br><strong>Typical Use Cases:</strong> Document digitisation, real‑time translation of street signs, automated invoice processing.<br>
<strong>Approaches:</strong> 
<ol>
  <li><strong>Text Detection:</strong> Locate text regions with models like EAST, CRAFT, or DBNet.</li>
  <li><strong>Text Recognition:</strong> Convert detected regions into text using CRNN, Rosetta, or TrOCR.</li>
</ol>
</p>


