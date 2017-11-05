---
layout: default
comments: true
title:  "Set up your Continuous Integration Locally"
date:   2017-05-11 19:55:01 +0100
categories: WCF
description: "A simple guide on how to install everything locally to get familiar with continuos integration"
---
# [](#header-1) Continuos Integration. A simple guide on how to install everything locally to get familiar with it. 

For this test you can use your local machine, a remote server or a virtual machine. 

I decided to install all this tools on a virtual machine running Windows Server 2016 using Virtual Box. Virtual Box is running on my local Win10 host machine.

## [](#header-3) Install a Git Repository Server

As a git repo server I decided to try Go Git Service (Gogs https://gogs.io/). 

How to install gogs:

1) Install Git https://git-scm.com/download/win

2) Install mysql server (I installed the 5.7 version) https://www.mysql.com/downloads/

2a) In order to install mysql you need to install Install Visual C++ Redistributable Packages for VS 2013 https://www.microsoft.com/en-ie/download/details.aspx?id=40784 (this is required by mysql)

2b) NOTE: If the installation fails you need to install the following update https://support.microsoft.com/en-ie/help/3179560/update-for-visual-c-2013-and-visual-c-redistributable-package

Once mySQL is installed you need to create a database named gogs

![mysql screenshot]({{ site.url }}/assets/CI/CaptureMySql1.PNG)

![mysql screenshot]({{ site.url }}/assets/CI/CaptureMySql1.PNG)

3) Install gogs: download the binary: https://gogs.io/docs/installation/install_from_binary, unzip it and run it  

![gogs screenshot]({{ site.url }}/assets/CI/gogs.PNG)

4) navigate to 127.0.0.1:3000, insert your mysql password and click install

![gogs screenshot]({{ site.url }}/assets/CI/install.PNG)

5) create an account and create your first repo

![gogs screenshot]({{ site.url }}/assets/CI/account.PNG)

![gogs screenshot]({{ site.url }}/assets/CI/repo.PNG)

## [](#header-3) Virtual Box Settings

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
