---
title:  "Deploying Dotnet Core on AWS Linux"
date:   2019-05-21 15:04:23
categories: [dotnercore]
tags: [dotnercore, linux, aws]	
---
In this post, I will be sharing in detail my experience of hosting Asp.Net Core Web Api on AWS EC2 Linux instance. Some of the steps are detailed and you might want to skip those steps.

<h3>Step 1: Create a DotNet Core Web Api project</h3>

<p>Start Visual Studio 2017 and click on File -> New -> Project and then select Web -> Asp.Net Core Web Application from the templates and click Ok. </p>
 <img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/1-create-dotnet-core-project.PNG" class="fullsize-image" alt="-----">

 <p>Next choose API from the options and click Ok. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/2-select-project-type.PNG" class="fullsize-image" alt="-----">

<p>Now we will have the project created. I am not making any changes to the code and will leave ValuesController as it is so api/values will return value1 and value2. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/3-code-view.PNG" class="fullsize-image" alt="-----">

<p>Run the application and it should display results from the api/values call. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/4-api-result.PNG" class="fullsize-image" alt="-----">

<h3>Step 2: Create a deployment package</h3>

<p>Let's create the deployment package which we will copy to EC2 instance. </p>

<p>Right click WebApiCore project and select Publish. This will bring up the publishing dialog. </p>

<p>From right menu, pick Folder as publishing target. Leave folder location in Choose a folder as it is. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/5-publish-target.PNG" class="fullsize-image" alt="-----">

<p>Click Advanced, and then set the options as below and hit Save. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/6-publish-advance-setting.PNG" class="fullsize-image" alt="-----">

<p>Now click on Create Profile at at the bottom of the dialog. This will create a FolderProfile. </p>

<p>Hit the Publish button. </p>

<p>Once finished, check the Target Location to find publish artefacts, which I will use for deployment. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/7-publish-artifacts.PNG" class="fullsize-image" alt="-----">


<h3>Step 3: Create AWS Linux EC2 Instance</h3>
<p>Let's create an EC2 instance by logging on to AWS management console at https://console.aws.amazon.com/console/home. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/8-aws-console.PNG" class="fullsize-image" alt="-----">

<p>Go to EC2 dashboard and click on Launch Template. Search for Ubuntu Server 18.04 and select Ubuntu Server 18.04 LTS (HVM), SSD Volume Type (you can select other images as well, I just used this one). </p>
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

<p> A key pair dialog will be displayed. Choose Create a New Key Pair and give it a meaningful name and download to local disk. The key is crucial for connecting to EC2 instance and we will be using it later in Step 4: Login to Linux EC2 Instance. After downloading the key pair, click Launch Instances. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/16-launch-instance.PNG" class="fullsize-image" alt="-----">

<p>Go to EC2 -> Instances view to see the list of launched instances. Our instance should be there. Please note the Public DNS as it will be used in Step 4: Login to Linux EC2 Instance. </p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/17-instance-list.PNG" class="fullsize-image" alt="-----">


<h3>Step 4: Login to Linux EC2 Instance</h3>
<p>There are several ways to connect to Linux instance. I will be using PuTTy. Please go ahead and download from <a href="https://www.chiark.greenend.org.uk/~sgtatham/putty/" target="_blank">here</a> </p>

<p>First thing after installing PuTTY is transform private key (downloaded in previous step) into the format that PuTTY can understand. To do so, we will be using a utilty PuTTYgen. This utility is included with PuTTY and should be installed on your system.</p>

<p>Open PuTTYgen, and then choose the SSH-2 (or SSH-1) RSA option to specify the type of key that you want to generate. Next, click the Load button and select your private key file (you will need to choose the All Files option in order to get the utility to display .PEM files). Now, click the Save Private Key (not Save Public Key) button as shown below (Don't worry about the message indicating that a pass phrase is not being used. Just specify a name for the private key file that is being created)</p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/18A-putty-gen.PNG" class="fullsize-image" alt="-----">

<p>Now launch PuTTY. The first thing that we will need to enter is the host name. In our case it will be ubuntu@{public dns name of instance}. Please insert the public dns name of instance noted in previous step as below.</p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/18-putty-host-name.PNG" class="fullsize-image" alt="-----">

<p>Now, navigate through the PuTTY console tree to Connection | SSH | Auth. Click the Browse button, select the private key file that we just created, and then click Open. It should looks like below.</p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/19-putty-auth.PNG" class="fullsize-image" alt="-----">

<p>You should be connected and logged in to your instance as below.</p>
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/20-putty-logged-in.PNG" class="fullsize-image" alt="-----">
 
<h3>Step 5: Prepare the machine for DotNet Core</h3>
<p>It's always good idea to update the instance to latest packages with following commands:
<pre><code>
<span>$</span> sudo apt-get update
<span>$</span> sudo apt-get upgrade
<span>$</span> sudo apt-get dist-upgrade
</code></pre>
</p>
<p>First step is to install dotnet core. Now, before installing, you should first check which dotnet core version is used in your application. You can check by running below command in the command line of machine where you are building your application.

<pre><code>
<span>$</span> dotnet --version
</code></pre>

My version is 2.1.200. I will install this version in the instance.
</p>

<p>First, you’ll need to register the Microsoft key and product repository, and install required dependencies. This only needs to be done once per machine. Run the following commands on your PuTTY terminal.

<pre><code>
<span>$</span> wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
<span>$</span> sudo dpkg -i packages-microsoft-prod.deb
</code></pre>
</p>

<p>Now install dotnet core with the following commands. 
<pre><code>
<span>$</span> sudo add-apt-repository universe
<span>$</span> sudo apt-get install apt-transport-https
<span>$</span> sudo apt-get update
<span>$</span> sudo apt-get install dotnet-sdk-2.1.200
<span>$</span> dotnet --version # to check dotnet core installed correctly
</code></pre>
</p>

<p>
Let's now install nginx as reverse proxy for our application. It will be equivalent of iis on windows. We will use apt-get to install Nginx. The installer creates a systemd init script that runs Nginx as daemon on system startup. We will run the command below:
<pre><code>
<span>$</span> sudo -s
<span>$</span> nginx=stable # use nginx=development for latest development version
<span>$</span> add-apt-repository ppa:nginx/$nginx
<span>$</span> apt-get update
<span>$</span> apt-get install nginx
</code></pre>
</p>

<p>
Since Nginx was installed for the first time, we have to explicitly start it by running:
<pre><code>
<span>$</span> sudo service nginx start
</code></pre>
</p>

<p>
We can verify the installation from browser. Nginx landing page should be reachable at http://server_IP_address/index.nginx-debian.html or http://server_IP_address/ (you can use public ip address or public dns ). If it looks as below then your installation is successfull:
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/21-nginx-index-page.PNG" class="fullsize-image" alt="-----">
</p>

<p>
Lets configure Nginx as a reverse proxy to forward web requests to our ASP.NET Core app (which will be hosted at http://localhost:5000), we need to modify /etc/nginx/sites-available/default. Let’s open it in a text editor, and replace the contents with the following:
<pre><code>
server {
 listen 80;
 location / {
 proxy_pass http://localhost:5000;
 proxy_http_version 1.1;
 proxy_set_header Upgrade $http_upgrade;
 proxy_set_header Connection keep-alive;
 proxy_set_header Host $host;
 proxy_cache_bypass $http_upgrade;
 proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
 proxy_set_header X-Forwarded-Proto $scheme;
 }
}
</code></pre>

Nginx is listening to port 80. We have enabled the port 80 on the EC2 instance in Step 3: Create AWS Linux EC2 Instance.
</p>

<p>
Once the Nginx configuration is established, we have to run following command to verify the syntax of the configuration files. 
<pre><code>
<span>$</span> sudo nginx -t
</code></pre>
</p>

<p>
If the configuration file test is successful, we will force Nginx to pick up the changes by running:
<pre><code>
<span>$</span> sudo nginx -s reload
</code></pre>
</p>
<p>
At this point, http://13.239.115.224(or http://13.239.115.224/index.nginx-debian.html) will show 502 Bad Gateway status. This is because our application is not deployed and running.
</p>

<h3>Step 6: Deploy and run your application</h3>
<p>
Let create a new directory on EC2 instance to deploy our code into (also change permission to give write access):
<pre><code>
<span>$</span> sudo mkdir /var/www/api
<span>$</span> sudo chmod 777 /var/www/api
</code></pre>
</p>

<p>Now, we will copy our application code to the EC2 instance. To copy our application, we will install WinSCP (WinSCP is a popular SFTP client and FTP client for Microsoft Windows). Please download and install from <a href="https://winscp.net/eng/download.php" target="_blank">here</a> 
</p>

<p>
Now, let’s connet to our EC2 instance with WinSCP. Fill in the hostname and user name as below:
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/22.winscp-hostname.PNG" class="fullsize-image" alt="-----">
</p>

<p>
Next click on Advanced and choose Advanced from drop down to bring the Advanced Site Setting dialog as below:
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/23-winscp-advanced-site-setting.PNG" class="fullsize-image" alt="-----">
</p>

<p>
Select SSH -> Authentication and click ... to bring Select private key file dialgon as below:
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/24-winscp-select-private-key.PNG" class="fullsize-image" alt="-----">
</p>

<p>
Select the private key created at the end of Step 3: Create AWS Linux EC2 Instance. Click Ok to close the dialog and then click Login to connect to EC2 instance. 
After connecting successfully, we can see below screen:
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/25-winscp-files-view.PNG" class="fullsize-image" alt="-----">
</p>

<p>
On the left is my local computer files showing the directory where we published the api code in Step 2: Create a deployment package. On the right is /var/www/api/ directory of my remote computer showing the files of the EC2 instance (you have to navigate to this folder). 
</p>

<p>
Select all the files in left and click upload. Once completed, directory of remote computer like below:
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/26-winscp-uploaded.PNG" class="fullsize-image" alt="-----">
</p>

<p>
Now navigate to /var/www/api/ directory on the remote machine and run the following command:
<pre><code>
<span>$</span> dotnet WebApiCore.dll
</code></pre>

This should produce the log below:
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/27-putty-run-dotnet.PNG" class="fullsize-image" alt="-----">
</p>

<p>
Now go to chrome and navigate to http://13.239.115.224/api/values and you will see the following view:
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/28-api-running.PNG" class="fullsize-image" alt="-----">

This means our api is running successfull on Linux EC2 instance and is being served by Nginx.
</p>
<h3>Step 7: Running your application as service</h3>
<p>
But, if we close the PuTTY session, our application will stop. But, we don’t want that. We have to use systemd (systemd is an init system that provides many powerful features for
starting, stopping, and managing processes) to create a service file to start and monitor the underlying web app.

At first, we will create a service file for our web application.
<pre><code>
<span>$</span> sudo nano /etc/systemd/system/web-api-core.service
</code></pre>
</p>

<p>
Then, inside that web-api-core.service file, i have included below configurations:
<pre><code>
[Unit] 
Description=DotNet Core application on Ubuntu 

[Service] 
WorkingDirectory=/var/www/api 
ExecStart=/usr/bin/dotnet /var/www/api /WebApiCore.dll 
Restart=always 
RestartSec=10 # Restart service after 10 seconds if dotnet service crashes 
SyslogIdentifier=offershare-web-app
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install] 
WantedBy=multi-user.target
</code></pre>
</p>

<p>
Then, we need to enable the service and run it.
<pre><code>
<span>$</span> sudo systemctl enable web-api-core.service
<span>$</span> sudo systemctl start web-api-core.service
<span>$</span> sudo systemctl status web-api-core.service
</code></pre>
</p>

<p>
The status command should show the service as running as below: 
<img src="{{ site.baseurl }}/images/blog/setting-up-dtnet-core-linux/29-service-running.PNG" class="fullsize-image" alt="-----">
</p>
<p>
So, our web application is running. Kestrel by default listens on port 5000, so our application is available on http://localhost:5000.
</p>

<p>
Now, if we close the PuTTY session, our web application is still running.
</p>
