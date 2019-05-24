---
title:  "Deploying Dotnet Core on AWS Linux"
date:   2019-05-21 15:04:23
categories: [dotnercore]
tags: [dotnercore, linux, aws]
---
In this post, I will share in details my experience of how to host Asp.Net Core Web Api on AWS EC2 Instance. Some of the steps are too detailed and it might feel like I am teaching a beginner, please skip those if you already know.

<h4>Step 1: Create a DotNet Core Web Api project</h4>

<p>Fire up Visual Studio 2017 and click on File -> New -> Project. </p>
 <img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/1-create-dotnet-core-project.PNG" class="fullsize-image" alt="-----">
<p>Then select Web -> Asp.Net Core Web Application from the templates. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/2-select-project-type.PNG" class="fullsize-image" alt="-----">
<p>Next step choose API from the options. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/2-select-project-type.PNG" class="fullsize-image" alt="-----">
<p>Now we will have the project created. I will not be making any changes to the code and will leave ValuesController as it is so api/values will return value1 and value2. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/3-code-view.PNG" class="fullsize-image" alt="-----">

<p>Run the application and you it will display results from the api/values call. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/4-api-result.PNG" class="fullsize-image" alt="-----">

<h4>Step 2: Create a deployment package</h4>

<p>Let's create the deployment package which will be copied to ec2 instance. </p>

<p>Right click WebApiCore project and select Publish. This will bring up the publishing dialog. </p>


<p>From right menu, pick Folder as publishing target. Leave folder location in Choose a folder as it is. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/5-publish-target.PNG" class="fullsize-image" alt="-----">

<p>Click Advanced, and then set the options as below and hit Save. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/6-publish-advance-setting.PNG" class="fullsize-image" alt="-----">

<p>Now click on Create Profile at at the bottom of the dialog. This will create a FolderProfile. </p>

<p>Hit the Publish button. </p>

<p>Once finished, check the Target Location to find publish artefacts, which I will use for deployment. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/7-publish-artifacts.PNG" class="fullsize-image" alt="-----">


<h4>Step 3: Create AWS Linux EC2 Instance and login to your machine</h4>

<h4>Step 4: Prepare the machine for DotNet Core</h4>

<h4>Step 5: Deploy and run your application</h4>

<h4>Step 6: Running your application as service</h4>



