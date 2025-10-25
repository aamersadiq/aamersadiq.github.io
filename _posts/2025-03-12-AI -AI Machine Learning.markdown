---
title: "AI - Machine Learning"
date: 2025-03-12 09:33:43
categories: [ai, machine-learning]
tags: [ai, machine-learning]
---

I recently completed the <a href="https://learn.microsoft.com/api/credentials/share/en-us/AamerSadiq-9608/90234AA9C5746E38?sharingId=4D3B72AC4C8C2E1D" target="_blank">Azure AI Fundamentals</a> certification,
giving me a solid foundation in understanding AI concepts and tools, which I am excited to put into practice.
In this blog, I want to share the knowledge I've gained, starting with the fundamental concepts of machine learning.
Let's get started.

<h3>Machine Learning</h3>
Artificial Intelligence (AI) refers to the broader concept of machines or software systems designed to replicate human intelligence, enabling them to carry out tasks, make decisions, and solve complex problems.
Machine learning is an area of artificial intelligence (AI) that enables systems to learn from data, identify patterns, and make decisions or predictions without being explicitly programmed. Instead of following fixed instructions, machine learning models improve their performance over time as they are exposed to more data.
Machine learning is based on statistics and mathematical data modeling, where the main idea is to use historical data to predict future outcomes or unknown values.
A machine learning model is essentially acts as a function to compute an output value (<span style="font-weight: bold;">label</span>) based on given input values (<span style="font-weight: bold;">features</span>). Developing this function is known as the <span style="font-weight: bold;">training</span> process, which involves analyzing historical data to identify patterns and relationships. Once trained, the model can be used to make predictions on new data in a process called <span style="font-weight: bold;">inferencing</span>.

The training dataset typically contains past observations that include features (the attributes or characteristics of the data) and their corresponding labels (the known value to be predicted). Mathematically:

<ul>
    <li>Features are often represented as <span style="font-weight: bold;">x</span> (sometimes as a vector of multiple values, like [x₁, x₂, x₃, ...]).</li>
    <li>The label, or target value, is denoted as <span style="font-weight: bold;">y</span>.</li>
    <li>An algorithm is applied to the data to establish a relationship between the features (x) and the label (y). This relationship is then generalized as a function that predicts y based on x. For example, if the algorithm outputs a function f, we can express the prediction as: <span style="font-weight: bold;">y = f(x)</span></li>
</ul>

Once trained, the model becomes capable of predicting new labels for unseen data. Since these predictions are derived from the function rather than actual observations, the predicted value is often represented as <span style="font-weight: bold;">ŷ</span> (pronounced “y-hat”).

<h3>Types of Machine Learnings </h3>

<img src="{{ site.baseurl }}/images/blog/ai-basics/types-of-machine-learnings.png" class="fullsize-image" alt="Image">

Machine learning can be broadly categorized into three types, each serving unique purposes and leveraging distinct approaches.

<h4>Supervised Learning:</h4>
Supervised learning involves training a model on labeled data, where the output is already known. The algorithm learns by comparing its predictions to the actual outcomes and adjusts accordingly. Supervised learning can be separated into two types:

<span style="font-weight: bold;">Regression:</span>
Predicting continuous numerical values. Examples:

<ul>
    <li>Weather forecasting: predicting temperature or rainfall.</li>
    <li>Stock price prediction: estimating future prices based on market trends.</li>
    <li>Sales forecasting: predicting monthly revenue for a retail store.</li>
    <li>Healthcare: predicting patient recovery time based on treatment plans.</li>
    <li>Real estate: estimating housing prices based on property features.</li>
</ul>

<span style="font-weight: bold;">Classification:</span>
Categorizing data into discrete groups. It can be subdivided into binary and multi classification. Examples:

<ul>
    <li>Email filtering: categorizing emails as spam or not spam.</li>
    <li>Fraud detection: identifying fraudulent credit card transactions.</li>
    <li>Medical diagnosis: predicting the presence of a disease based on test results.</li>
    <li>Image recognition: classifying objects in photographs.</li>
    <li>Customer feedback analysis: sorting reviews as positive or negative.</li>
</ul>

<h4>Unsupervised Learning:</h4>
Unsupervised learning deals with unlabeled data, where the algorithm identifies patterns without pre-defined outputs. Unsupervised learning can be separated into two types:

<span style="font-weight: bold;">Clustering:</span>
Groups data points into clusters based on similarity. Examples:

<ul>
    <li>Customer segmentation: grouping customers based on purchasing behavior.</li>
    <li>Social network analysis: identifying communities within a network.</li>
    <li>Wildlife monitoring: grouping animal species based on migration patterns.</li>
    <li>Document organization: categorizing documents into similar topics.</li>
    <li>Market research: identifying trends among survey responses.</li>
</ul>

<span style="font-weight: bold;">Dimensionality Reduction:</span>
Simplifies datasets by removing redundant features. Examples:

<ul>
    <li>Image compression: reducing the number of pixels while retaining essential information.</li>
    <li>Data visualization: projecting high-dimensional data into 2D/3D for better understanding.</li>
    <li>Genomics: reducing the dimensionality of genetic data for disease analysis.</li>
    <li>Speech processing: removing noise from audio signals while preserving clarity.</li>
    <li>Feature selection: identifying important variables in large datasets for machine learning.</li>
</ul>

<h4>Reinforcement Learning:</h4>
Decision making to maximize rewards. Examples:
<ul>
    <li>Self-driving cars: learning optimal driving strategies.</li>
    <li>Game AI: training AI agents to play strategy games like chess or Go.</li>
    <li>Robotics: programming robots to navigate environments or perform tasks.</li>
    <li>Dynamic pricing: adjusting prices based on market demand.</li>
    <li>Energy optimization: controlling smart grids to reduce electricity consumption.</li>
</ul>

<h3>Common Machine Learning Models</h3>
Machine learning models are designed to solve specific types of problems, and understanding their applications is key to selecting the right model for a task. Here are some popular models, along with examples of how they’re used:
<ul>
    <li><span style="font-weight: bold;">Linear Regression: </span>Ideal for predicting continuous values. Example: Forecasting house prices based on factors like square footage, location, and amenities.</li>
    <li><span style="font-weight: bold;">>Logistic Regression: </span>A go-to model for binary classification tasks. Example: Predicting whether a customer will make a purchase or not based on their online behavior.</li>
    <li><span style="font-weight: bold;">Decision Trees: </span>Useful for interpretable and straightforward decisions. Example: Diagnosing medical conditions based on symptoms, lab tests, and patient history.</li>
    <li><span style="font-weight: bold;">Random Forests: </span>Excellent for handling complex datasets. Example: Predicting credit risk by analyzing multiple factors like income, debt, and payment history.</li>
    <li><span style="font-weight: bold;">Support Vector Machines (SVMs): </span>Effective for classification problems with clear boundaries. Example: Detecting fraudulent transactions in financial data.</li>
    <li><span style="font-weight: bold;">K-Means Clustering: </span>Commonly used for segmentation. Example: Grouping customers into clusters based on purchase patterns to target marketing campaigns.</li>
    <li><span style="font-weight: bold;">Neural Networks: </span>Powerful models for unstructured data like images, audio, and text. Example: Recognizing handwritten digits in postal codes or detecting objects in photos.</li>
    <li><span style="font-weight: bold;">Gradient Boosting Machines (e.g., XGBoost): </span>Widely used for structured data applications. Example: Predicting loan approval by considering factors such as credit score, income, and existing debt.</li>
</ul>
