---
layout: default
comments: true
title:  "Configuration in .NET Core 2.2 Console App using HostBuilder"
date:   2019-05-11 19:55:01 +0100
categories: .NET Core
description: "Configuration in .NET Core 2.2 Console App using HostBuilder"
---
# [](#header-1) Configuration in .NET Core 2.2 Console App using HostBuilder

Similar to the WebHostBuilder class that allows to build a WebHost which hosts a web application, since .Net core 2.1, for non http scenarios, a generic HostBuilder has been available, and allows us to build an Host (IHost) that hosts services. 

In my previous post <a href="https://maciti.github.io/.net/core/2019/03/23/Configuration-Settings-Net-Core-Console.html">Configuration in .NET Core 1.0 Console Application</a> I described how to load application settings from a json file in a console app written in .net core 1.0. 

In this post, using .Net core 2.2, I tried to do the same using the HostBuilder.

## [](#header-3) Add an AppSettings.json file

After a Console App (.Net Core > 2.1) is created, add a json file. I called it appsettings.json. Add some key/values and in its properties select "Copy if newer" for "Copy to Output Directory"

![app settings]({{ site.url }}/assets/Console/appsettings3.JPG)

![app settings]({{ site.url }}/assets/Console/appsettings.JPG)

## [](#header-3) The HostBuilder

Using nuget I installed the following packages:

- Microsoft.Extensions.Hosting,
- Microsoft.Extensions.Configuration, 
- Microsoft.Extensions.Configuration.Json,
- Microsoft.Extensions.Options, 
- Microsoft.Extensions.Options.ConfigurationExtensions

I created a class in order to bind my configuration section against an object (note that property names must match config names)

{% highlight csharp %}
    public class RabbitMqConfiguration
    {
        public string Uri { get; set; }

        public string VirtualHost { get; set; }

        public string User { get; set; }

        public string Password { get; set; }
	
    }
{% endhighlight %}

I will use the HostBuilder to build an Host (IHost). In Main using the ConfigureAppConfiguration method of the HostBuilder I'll set up the configuration, and using the ConfigureServices method I'll add services to the app's dependency injection container.

In the ConfigurationServices method I registered a configuration instance where I bound the RabbitMq configuration section against the RabbitMqConfiguration class. I also registered a hosted service.

{% highlight csharp %}
	public static async Task Main(string[] args)
	{
		var host = new HostBuilder().ConfigureAppConfiguration((hostContext, configApp) =>
		{
			configApp.AddJsonFile("appsettings.json", optional: true);
		})
		.ConfigureServices((hostContext, services) =>
		{
			services.Configure<RabbitMqConfiguration>(hostContext.Configuration.GetSection("RabbitMq"));
			services.AddHostedService<TestService>();
		})
		.Build();

		await host.RunAsync();
	}
{% endhighlight %}

## [](#header-3) The Hosted Service

The idea is that the hosted service is a background task hosted in the service container. The hosted service has to implement the IHostedService interface, implementing StartAsync and StopAsync methods.

the host is responsible to start and stop all the services that are registered in the service container.

I created a simple service that prints in console a config value. The service receives the configuration object once is created through dependency injection. note: check the option pattern <a href="https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-2.2">pattern</a>

{% highlight csharp %}
    public class TestService : IHostedService
    {
        private readonly RabbitMqConfiguration _rabbitConfiguration;

        public TestService(IOptions<RabbitMqConfiguration> options)
        {
            _rabbitConfiguration = options.Value;
        }

        public async Task StartAsync(CancellationToken cancellationToken)
        {
            await Console.Out.WriteLineAsync("starting service..");
            await Console.Out.WriteLineAsync($"config user value: {_rabbitConfiguration.User}");
        }

        public async Task StopAsync(CancellationToken cancellationToken)
        {
            await Console.Out.WriteLineAsync("stopping service..");
        }
    }
{% endhighlight %}

Running the app I can see my config setting printed in console.
 

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/.net/core/2019/05/11/Configuration-Settings-Net-Core-22-Console.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2019-05-11-Configuration-Settings-Net-Core-22-Console'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
