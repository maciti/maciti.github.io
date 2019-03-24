---
layout: default
comments: true
title:  "Configuration in .NET Core Console App"
date:   2019-03-23 19:55:01 +0100
categories: .NET Core
description: "Configuration in .NET Core Console App using appsettings.json"
---
# [](#header-1) Configuration in .NET Core Console Application

The first time I created a console application in .net core I had some troubles finding a simple documentation on how to use a json application settings file and retrieve configuration values from it.

In the following 3 simple steps I will explain how to do it..

## [](#header-3) Add an AppSettings.json file

After your Console App (.Net Core) is created, add a json file. I called it appsettings.json. Add some key/values and in its properties select "Copy if newer" for "Copy to Output Directory"

![app settings]({{ site.url }}/assets/Console/appsettings2.JPG)

![app settings]({{ site.url }}/assets/Console/appsettings.JPG)

## [](#header-3) Create a Settings Provider 

I created a class to build and provide an IConfiguration with keys and values

{% highlight csharp %}
    public class SettingsProvider
    {
        private static IConfiguration _configurationRoot;

        public static IConfiguration Configuration
        {
            get
            {
                if (_configurationRoot == null)
                {
                    BuildConfiguration();
                }

                return _configurationRoot;
            }
        }

        private static void BuildConfiguration()
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true);

            _configurationRoot = builder.Build();
        }
    }
{% endhighlight %}

## [](#header-3) Retrieve values

Simply call the static Configuration property..

{% highlight csharp %}
var uri = SettingsProvider.Configuration["RabbitMqUri"];
{% endhighlight %}
 

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/asp.net/core/2019/03/23/Configuration-Settings-Net-Core-Console.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2019-03-23-Configuration-Settings-Net-Core-Console'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
