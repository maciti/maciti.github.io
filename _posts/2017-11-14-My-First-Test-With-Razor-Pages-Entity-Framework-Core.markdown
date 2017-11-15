---
layout: default
comments: true
title:  "Razor pages and Entity Framework core as an In-Memory DB"
date:   2017-11-14 19:55:01 +0100
categories: Razor
description: "My first test with razor pages and Entity Framework core as an In-Memory Database"
---
# [](#header-1) Razor pages and Entity Framework core as an In-Memory DB. 

Recently I installed Visual Studio 2017 and I wanted to try Razor Pages, a new feature of ASP.NET Core 2. According to Microsoft it makes coding page-focused scenarios easier and more productive. 

For my test I decided to create a simple web page with a simple form where a user can subscribe to a website inserting name and email address. The same page will also show the list of users already registered. 

With the introduction of Entity Framework Core 1.0 it is possible to use an in-memory data provider, this will spare us to use a local or external database.

## [](#header-3) Installation

For this test I downloaded and installed VS 2017 community edition. <a href="https://www.visualstudio.com/downloads/">visual studio downloads</a>

and I downloaded and installed from github .net core 2.0 <a href="https://github.com/dotnet/core/blob/master/release-notes/download-archives/2.0.0-download.md">.net core 2.0</a>

## [](#header-3) Create new project

In Visual Studio 2017 create a new .NET Core Project..

![razor screenshot]({{ site.url }}/assets/RazorPages/newproj.PNG)

..select ASP.NET Core 2.0 and Web Application 

![razor screenshot]({{ site.url }}/assets/RazorPages/newproj2.PNG)

Now the project has been created. Via Nuget packages I installed Microsoft.EntityFrameworkCore.InMemory

![razor screenshot]({{ site.url }}/assets/RazorPages/newproj3.PNG)

## [](#header-3) Create the DB Model and the DBContext

I created a new folder called DbModels, and I created the first class representing the user subscribing the service

![razor screenshot]({{ site.url }}/assets/RazorPages/sub.PNG)

{% highlight csharp %}

    public class Subscriber
    {
        public int Id { get; set; }

        [Required]
        public string Name { get; set; }

        [Required]
        [RegularExpression(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$", ErrorMessage = "Email field must be a valid email address")]
        public string Email { get; set; }
    }

{% endhighlight %}

I created my DbContext for Entity Framework, it implements DbContext class, and it contains the entity set "Subscribers"

![razor screenshot]({{ site.url }}/assets/RazorPages/inmemory.PNG)

{% highlight csharp %}

    public class InMemoryDbContext : DbContext
    {
        public InMemoryDbContext(DbContextOptions<InMemoryDbContext> opts) : base(opts) { }

        public DbSet<Subscriber> Subscribers { get; set; }
    }

{% endhighlight %}

The last step is to initialize the DbContext in the Startup.cs file, adding the following line inside the ConfigureServices method

{% highlight csharp %}

services.AddDbContext<InMemoryDbContext>(options => options.UseInMemoryDatabase("myFirstInMemDB"));

{% endhighlight %}

## [](#header-3) Create new Razor Page

Righ click on pages folder and add a new Razor Page. I called this page "Subscribe"

![razor screenshot]({{ site.url }}/assets/RazorPages/add.PNG)

![razor screenshot]({{ site.url }}/assets/RazorPages/rp.PNG)

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/razor/2017/11/14/My-First-Test-With-Razor-Pages-Entity-Framework-Core.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2017-11-14-My-First-Test-With-Razor-Pages-Entity-Framework-Core'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
