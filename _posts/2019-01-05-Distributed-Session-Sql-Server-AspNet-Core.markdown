---
layout: default
comments: true
title:  "Distributed Session in AspNet Core (2.1) using Sql Server"
date:   2019-01-05 19:55:01 +0100
categories: ASP.NET Core
description: "how to use the SQL Server cache implementation to store sessions in SQL server in a distributed environment"
---
# [](#header-1) Distributed Session in AspNet Core using Sql Server

In a distributed environment using the SQL Server cache implementation we can store session values in SQL server.

For more info about distributed caching in asp.net core: <a href="https://docs.microsoft.com/en-us/aspnet/core/performance/caching/distributed?view=aspnetcore-2.1#distributed-sql-server-cache">distributed caching in asp.net core</a> 

## [](#header-3) Create a SESSION table

note: you need to have .net sdk 2.1 installed.

you can run the following command using command prompt to create table and index for your new session table (this will create a TestSession table under Test DB)

{% highlight csharp %}
dotnet sql-cache create "Data Source=.\SQLEXPRESS;Initial Catalog=Test;Integrated Security=True" dbo TestSession
{% endhighlight %}

I'm going to create a new login/user (security -> new login). I will need this user in order to read and write in the new Session Table.

![sql create new user screenshot]({{ site.url }}/assets/DistributedSession/CaptureSessionUser.JPG)

![sql create new user 2 screenshot]({{ site.url }}/assets/DistributedSession/CaptureSessionUser2.JPG)

here I'm giving to the new user the permission to select/update/write/delete on the TestSession table. 

![sql create new user 3 screenshot]({{ site.url }}/assets/DistributedSession/CaptureSessionUser3.JPG)

## [](#header-3) Create a MVC web app 

Create a new project (ASP.NET core Web Application) selecting NET.Core 2.1 as target Framework and ASP.NET MVC as project template.

Using Nuget install the following packages: Microsoft.Extensions.Caching.SqlServer, Microsoft.Extensions.Caching.SqlConfig.Tools,Microsoft.AspNetCore.Session

![nuget package screenshot]({{ site.url }}/assets/DistributedSession/CaptureNuget.JPG)

![nuget package screenshot3]({{ site.url }}/assets/DistributedSession/CaptureNuget3.JPG)

![nuget package screenshot2]({{ site.url }}/assets/DistributedSession/CaptureNuget2.JPG)

In the appsettings.json add the connection string:

{% highlight csharp %}
	"ConnectionStrings": {
		"SqlSession": "Server=localhost\\SQLEXPRESS;Database=Test;Integrated Security=False;User Id=sessionUser;"
	}
{% endhighlight %}

better not to add the Password, otherwise it will end up in your version control repository.

You can use secret manager to store the password:

![secret manager screenshot]({{ site.url }}/assets/DistributedSession/secretmanager.png)

in the secret.json insert the password

{% highlight csharp %}
{
	"DbPasswords": {
		"DistributedSessionPassword": "your pwd bla bla.."
	}
}
{% endhighlight %}

In the startup.cs, in the ConfigureServices method add the Distributed Sql Server Cache:


{% highlight csharp %}

var sqlSessionConnString = new SqlConnectionStringBuilder(Configuration.GetConnectionString("SqlSession"));

sqlSessionConnString.Password = Configuration["DbPasswords:DistributedSessionPassword"];
           
services.AddDistributedSqlServerCache(options =>
            {
                options.ConnectionString = sqlSessionConnString.ConnectionString; 
                options.SchemaName = "dbo";
                options.TableName = "TestSession";
            }); 
			
services.AddSession();
			
{% endhighlight %}

In the Configure method insert  app.UseSession();

{% highlight csharp %}
app.UseSession();
{% endhighlight %}

to test it in any action of a controller save and retrieve a session value

{% highlight csharp %}
public IActionResult Index()
{
    HttpContext.Session.SetString("TestMessage", "Hello Session!");
    var message = HttpContext.Session.GetString("TestMessage");

    return View();
}
{% endhighlight %}

you should be able to see now a new entry in the TestSession table

![test screenshot]({{ site.url }}/assets/DistributedSession/CaptureTest.JPG)

To store and retrieve complex objects you need to serialize and deserialize, you can use JsonConvert class for example.

{% highlight csharp %}

HttpContext.Session.SetString("YourKey", JsonConvert.SerializeObject(value));

{% endhighlight %}
 

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/asp.net/core/2019/01/05/Distributed-Session-Sql-Server-AspNet-Core.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2019-01-05-Distributed-Session-Sql-Server-AspNet-Core'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
