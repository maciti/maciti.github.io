---
layout: default
comments: true
title:  "Set up your Continuous Integration Locally"
date:   2017-11-05 19:55:01 +0100
categories: WCF
description: "A simple guide on how to install everything locally to get familiar with continuos integration"
---
# [](#header-1) Continuos Integration. A simple guide on how to install everything locally to get familiar with it. 

For this test you can use your local machine, a remote server or a virtual machine. 

I decided to install all this tools on a virtual machine running Windows Server 2016 using Virtual Box. Virtual Box is running on my local Win10 host machine.

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

<b>now you should be able to clone your first repo locally, add a new file and push it!</b>

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/c-sharp/2017/11/05/Set-up-your-Continuos-Integration-Locally.html';  // Replace PAGE_URL with your page's canonical URL variable
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
