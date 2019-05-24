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




<h4>Step 3: Create AWS Linux EC2 Instance</h4>
<p>Let's create a ec2 instance by logging on to AWS management console at https://console.aws.amazon.com/console/home. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/8-aws-console.PNG" class="fullsize-image" alt="-----">

<p>Go to EC2 dashboard and click on Launch Template. Search for Ubuntu Server 18.04 and Select Ubuntu Server 18.04 LTS (HVM), SSD Volume Type (you can select any other image). </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/9-search-ami.PNG" class="fullsize-image" alt="-----">

<p>Next choose instance type eligible for free tier. Click Configure Instance Details</p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/10-select-instance-type.PNG" class="fullsize-image" alt="-----">

<p>Leave everything in instance details as default. Click Add Storage. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/11-instance-details.PNG" class="fullsize-image" alt="-----">

<p>Leave everything in storage options as default. Click Add Tags. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/12-add-storage.PNG" class="fullsize-image" alt="-----">

<p>Create a new Name tag as below (or you can leave it empty). Then click Configure Security Groups.</p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/13-add-tags.PNG" class="fullsize-image" alt="-----">

<p>Make sure SSH and HTTP ports are opened for access as below. Click Review and Launch. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/14-configure-security-groups.PNG" class="fullsize-image" alt="-----">

<p>Review instance launch and hit Launch. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/15-launch.PNG" class="fullsize-image" alt="-----">


<p>Key pair dialog will be displayed. Choose Create a New Key Pair and give it a meaningful name. After downloading the key pair click Launch Instances. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/16-launch-instance.PNG" class="fullsize-image" alt="-----">

<p>Go to EC2 -> Instances view to see the list of launched instances. Our instance should be there. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/17-instance-list.PNG" class="fullsize-image" alt="-----">


<h4>Step 4: Login to your LInux EC2 Instance</h4>

<h4>Step 5: Prepare the machine for DotNet Core</h4>

<h4>Step 6: Deploy and run your application</h4>

<h4>Step 7: Running your application as service</h4>



