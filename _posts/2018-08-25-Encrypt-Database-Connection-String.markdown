---
layout: default
comments: true
title:  "Encrypt Database Connection String"
date:   2018-08-25 19:55:01 +0100
categories: MVC
description: "Encrypt Connection Strings section of the web.config"
---
# [](#header-1) Encrypt Database Connection String

Very often the database connection string is stored in the web.config. The conncetion string can include passwords and other information that shouldn't be available.

A good practice is to encrypt the connection strings section of the web config.

## [](#header-3) Encrypt Connection Strings section of the web.config

Using IIS manager click on your application and then click on MachineKey.

![machinekey screenshot]({{ site.url }}/assets/EncryptConfig/MachineKey.PNG)

Untick "Automatically generate at runtime" for Validation key and Decryption key and click on Generate Keys. IIS will need this keys to decrypt your encrypted config section.

![machinekey screenshot]({{ site.url }}/assets/EncryptConfig/GenerateKeys.PNG)

To Encrypt the ConnectionStrings section of the web config we need to use aspnet_regiis program. 

Open the "Visual Studio Tools" folder (type Visual Studio Tools in Search Windows) and open Developer Command Prompt for VS2013 (note: launch it as administrator)

 The command to encrypt your section is the following

{% highlight csharp %}

aspnet_regiis -pe "connectionStrings" -app "/yourappname" -prov "RsaProtectedConfigurationProvider"

{% endhighlight %}

where prov is the encrytpion provider, you can use DPAPIProtectedConfigurationProvider as well (it uses the windows data protection api), but it won't work on multiple servers.

If you want to encrypt the configuration section of the web.config using the physical directory (not virtual), use the -pef option:  

{% highlight csharp %}

aspnet_regiis -pef "connectionStrings" "C:\yourpath\yourapp" -prov "RsaProtectedConfigurationProvider"

{% endhighlight %}

To decrypt the encrypted section:

{% highlight csharp %}

aspnet_regiis -pd "connectionStrings" -app "/yourappname"

{% endhighlight %}


{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/mvc/2018/08/25/Encrypt-Database-Connection-String.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2018-07-25-Encrypt-Database-Connection-String'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
