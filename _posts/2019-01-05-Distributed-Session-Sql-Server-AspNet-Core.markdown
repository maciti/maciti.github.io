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

you can run the following command using command prompt to create table and index for your new session table

{% highlight csharp %}
dotnet sql-cache create "Data Source=.\SQLEXPRESS;Initial Catalog=Test;Integrated Security=True" dbo TestSession
{% endhighlight %}

This will create a TestSession table under Test DB.

I'm going to create a new login/user (security -> new login):

![sql create new user screenshot]({{ site.url }}/assets/DistributedSession/CaptureSessionUser.JPG)

![sql create new user 2 screenshot]({{ site.url }}/assets/DistributedSession/CaptureSessionUser2.JPG)

here I'm giving to the new user the permission to select/update/write/delete on the TestSession table. 

![sql create new user 3 screenshot]({{ site.url }}/assets/DistributedSession/CaptureSessionUser3.JPG)

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/mvc/2018/09/15/MVC-Handle-Controller-Level-Errors.html';  // Replace PAGE_URL with your page's canonical URL variable
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
