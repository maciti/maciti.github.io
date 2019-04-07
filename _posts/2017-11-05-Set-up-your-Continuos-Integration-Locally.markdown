---
layout: default
comments: true
title:  "Set up your Continuous Integration Locally"
date:   2017-11-05 19:55:01 +0100
categories: CI
description: "A simple guide on how to install everything locally to get familiar with continuos integration"
---
# [](#header-1) Continuos Integration. A simple guide on how to install everything locally to get familiar with it. 

For this test you can use your local machine, a remote server or a virtual machine. 

I decided to install all this tools on a virtual machine running Windows Server 2016 using Virtual Box. Virtual Box is running on my local Win10 host machine.

# [](#header-2) The Repository Server

## [](#header-3) Install a Git Repository Server

As a git repo server I decided to try Go Git Service (Gogs https://gogs.io/). 

<b>How to install gogs:</b>

<b>1)</b> Install Git <a href="https://git-scm.com/download/win">https://git-scm.com/download/win</a>

<b>2)</b> Install mysql server (I installed the 5.7 version) <a href="https://www.mysql.com/downloads/">https://www.mysql.com/downloads/<a>

<b>2a)</b> In order to install mysql you need to install Install Visual C++ Redistributable Packages for VS 2013 <a href="https://www.microsoft.com/en-ie/download/details.aspx?id=40784">link<a> (this is required by mysql)

<b>2b)</b> NOTE: If the installation fails you need to install the following update <a href="https://support.microsoft.com/en-ie/help/3179560/update-for-visual-c-2013-and-visual-c-redistributable-package">link</a>

Once mySQL is installed you need to create a database named gogs

![mysql screenshot]({{ site.url }}/assets/CI/CaptureMySql1.PNG)

![mysql screenshot]({{ site.url }}/assets/CI/CaptureMySql2.PNG)

<b>3)</b> Install gogs: download the binary: <a href="https://gogs.io/docs/installation/install_from_binary">https://gogs.io/docs/installation/install_from_binary</a>, unzip it and run it  

![gogs screenshot]({{ site.url }}/assets/CI/gogs.PNG)

<b>4)</b> navigate to 127.0.0.1:3000, insert your mysql password and click install

![gogs screenshot]({{ site.url }}/assets/CI/install.PNG)

<b>5)</b> create an account and create your first repo

![gogs screenshot]({{ site.url }}/assets/CI/account.PNG)

![gogs screenshot]({{ site.url }}/assets/CI/hello.PNG)

<b>Now GOGS is up and running!!!</b>

## [](#header-3) Machine Settings

If you have installed gogs on your remote server or in a virtual machine you need to create an inbound rule for gogs in your server/vm firewall to be able to navigate to the gogs repo from your client.

1)run wf.msc

2)create inbound rule: allow TCP connection on port 3000

![gogs screenshot]({{ site.url }}/assets/CI/tcp.PNG)

3) from your client machine check if you are connected: http://server-ip:3000

<b> NOTE: If you are using virtual box you need to set up a couple of things first: </b>

<b>1)</b> go to file > preferences > network > host only network > add new one  and insert the following values

![gogs screenshot]({{ site.url }}/assets/CI/vb1.PNG)

![gogs screenshot]({{ site.url }}/assets/CI/vb2.PNG)

<b>2)</b>on your VM settings, navigate to network and select host only adapted and the name of the adapter just created

![gogs screenshot]({{ site.url }}/assets/CI/vb3.PNG)

from your guest machine run ipconfig to determine the ip address, now from your host machine you should be able to access to the gogs repo http://guest-machine-ip:3000

## [](#header-3) Test the Repo

Clone your first repo http://server-ip:3000/{user}/{your-first-repo}.git

Add a new file locally and push it into the server

Create a new branch from master and push it..

![gogs screenshot]({{ site.url }}/assets/CI/xyz.PNG)

## [](#header-3) Run Gogs om startup

On windows lanch task scheduler and create a new task

On trigger tab select AtLogOn or AtStartup

![gogs screenshot]({{ site.url }}/assets/CI/TS.PNG)

On action tab point to gogs.exe and add "web" as input parameter

![gogs screenshot]({{ site.url }}/assets/CI/TS2.PNG)

# [](#header-2) The Deployment Automation Server

As a deployment automation & release server I dedided to use Octopus

## [](#header-3) Install Octopus Server

For this test I've installed octopus in the same remote machine I installed gog.

Octopus needs sql server to function, so I've intalled sql server express. After that I started the installation of octopus:

![octopus installation]({{ site.url }}/assets/Octopus/start.JPG)

Check "start a free trial", after 30 days you can still use it.. you just need to convert it into octopus community edition

![octopus installation]({{ site.url }}/assets/Octopus/db.JPG)

I created a db named Octopus. In the next step select the port where ocopus deploy server is going to accept connection. After that set up admin user and password

![octopus installation]({{ site.url }}/assets/Octopus/user.JPG)
 
..finally click install! 

![octopus installation]({{ site.url }}/assets/Octopus/install.JPG)

A windows service named OctopusDeploy is created, it should start automatically at windows startup. Note: you should backup your octopus masterkey, the key is used for data encryption, to show your master key run the following command from command prompt: octopus.server show-master-key

Now navigate to http://192.168.56.101:8888 (replace with your remote machine ip) and login with your admin credentials just created

As soon as you log in you are prompted with a setup wizard, click on "create your first environment"

![octopus installation]({{ site.url }}/assets/Octopus/first.JPG)

I called it "Staging"

![octopus installation]({{ site.url }}/assets/Octopus/second.JPG)

After that you need to add a deployment target

![octopus installation]({{ site.url }}/assets/Octopus/third.JPG)

In the next step you'll be prompted with a thumbprint and a form (used to specify the Tentacle details)

![octopus installation]({{ site.url }}/assets/Octopus/fourth.JPG)

We don't have a tentacle yet.. <b>Copy the thumbprint..</b> it's now time to install our first tentacle on a new machine (we will come back here <b>***</b>) 

## [](#header-3) Install a tentacle

Now that the octopus server is installed we need to install our first tentacle on a different machine. A tentacle is a lightweight agent service, it communicates with octopus server. Octopus server will be responsible to deploy packages to the machines where the tentacles are installed.

![tentacle]({{ site.url }}/assets/Octopus/tentacle.JPG)

For the tentacle machine I created a new virtual machine called Staging.

![octopus staging]({{ site.url }}/assets/Octopus/CStaging.JPG)

and from the octopus website I've downloaded and installed Octopus Tentacle

![octopus tentacle download]({{ site.url }}/assets/Octopus/Tentacle2.JPG)

When you install the tentacle using the wizard select "listening tentacle"

![octopus installation]({{ site.url }}/assets/Octopus/StorageTentacle.JPG)

![octopus installation]({{ site.url }}/assets/Octopus/ListeningTentacle.JPG)

In the next step we need to insert the thumbprint we copied before, the thumbprint is used by the tentacle to trust the Octopus server.
 
![octopus installation]({{ site.url }}/assets/Octopus/Thumbprint.JPG)

When the installation is completed a Tentacle windows service will be running..listening for incoming requests from the Octopus server.

Now we leave the staging machine and we go back to the point where we were creating our first deployment target <b>***</b>

![octopus installation]({{ site.url }}/assets/Octopus/fourth.JPG)

Insert the IP address of the machine where the tentacle is installed (I had to create a NAT network in virtual box to achieve this)

If the Tentacle can be reached you can create a deployment target, specify display name (a unique machine name), environment name and target roles:

![octopus installation]({{ site.url }}/assets/Octopus/Connect.JPG)

note: after your new deployement target is created you might need to update your calamari version.

# [](#header-2) The Build Server

## [](#header-3) Install Jenkins

note: I've installed Jenkins in the same machine where I installed OctopusDeploy.

Download and run the windows installer, the windows installer automatically runs Jenkins as a windows service. 

Navigate to localhost:8080 and follow the installation steps

![gogs screenshot]({{ site.url }}/assets/CI/Jenkins1.PNG)

![gogs screenshot]({{ site.url }}/assets/CI/Jenkins2.PNG)

![gogs screenshot]({{ site.url }}/assets/CI/Jenkins3.PNG)

![gogs screenshot]({{ site.url }}/assets/CI/Jenkins4.PNG)

## [](#header-3) Create your first project

Click on create new jobs. <b> note: in Gogs I created a new repository called OctopuTest where I published e simple MVC web project</b>

note: <b> before starting you need to have the VS build tools installed in your server:</b> https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2017

![gogs screenshot]({{ site.url }}/assets/Jenkins/Project.JPG)

I created a freestyle project called OctopusTest. In the next step I specified my repo url and the branches I want to build. The repo is public and it's in the same server, so I don't need any credentials in this case. <br/>
note: In case you use another repo server like GitHub you will need to provide authentication.

![gogs screenshot]({{ site.url }}/assets/Jenkins/Project.JPG)

In the build section add build step "Build VS proj using MSBuild" and specify the name of your solution.

![gogs screenshot]({{ site.url }}/assets/Jenkins/Build.JPG)

![gogs screenshot]({{ site.url }}/assets/Jenkins/VS2017.JPG)

the VS2017 version was specified in the global configuration tools:

![gogs screenshot]({{ site.url }}/assets/Jenkins/Default.JPG)

build now! The blue ball means that the last build was successful.

![gogs screenshot]({{ site.url }}/assets/Jenkins/Blue.JPG)

# [](#header-2) Put Everything Together 

Install octopack in your solution using nuget package manager. This will be needed to build a nuget package that octopus will use to deploy.

![gogs screenshot]({{ site.url }}/assets/Octopus/Octopack.JPG)

In Octopus create your first project

![gogs screenshot]({{ site.url }}/assets/Octopus/OctopusWebTest.JPG)

Click "Define your deployement process", then "Add Steps" and choose "Deploy package"

![gogs screenshot]({{ site.url }}/assets/Octopus/DP.JPG)

Specify process name, add role and specify package name (in this case the name of the solution)

![gogs screenshot]({{ site.url }}/assets/Octopus/DP2.JPG)

expanding "configure feature" you can specify where you want to install the package, at the moment I have only one environment (staging). When you add a new environment, e.g. UAT, you need to have the same folder configuration in UAT machine, otherwise you need to use variables.

![gogs screenshot]({{ site.url }}/assets/Octopus/CID.JPG)

After this we need to create an API Key to allow Jenkins to communicate with octopus.

![gogs screenshot]({{ site.url }}/assets/Jenkins/Key.JPG)

in Jenkins, in OctopusTest propject click configure, in the build step add the following command line instructions 

/p:RunOctoPack=true /p:OctoPackPackageVersion=1.1.${BUILD_NUMBER} /p:OctoPackPublishPackageToHttp=http://localhost/nuget/packages /p:OctoPackPublishApiKey=${OctopusApiKey}

you can find more info here <a href="https://octopus.com/docs/api-and-integration/jenkins">https://octopus.com/docs/api-and-integration/jenkins</a>

![gogs screenshot]({{ site.url }}/assets/Jenkins/CL.JPG)

download the command line tool https://octopus.com/downloads and extract it in jenkins machine. Add a build step "Execute Windows batch command" in order to create an octopus release after the package is created

"C:\Tools\Octo\Octo.exe" create-release --project OctoFX --version 1.1.%BUILD_NUMBER% --packageversion 1.1.%BUILD_NUMBER% --server http://localhost/ --apiKey %OctopusApiKey% --releaseNotes "Jenkins build [%BUILD_NUMBER%]"

![gogs screenshot]({{ site.url }}/assets/Jenkins/Build3.JPG)

and add the OctopusApiKey in Credentials

![gogs screenshot]({{ site.url }}/assets/Jenkins/cred.png)

![gogs screenshot]({{ site.url }}/assets/Jenkins/Secret.JPG)

TO COMPLETE







{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/ci/2017/11/05/Set-up-your-Continuos-Integration-Locally.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2017-11-05-Set-up-your-Continuos-Integration-Locally'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://maciti-github-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
  
{% endif %}
