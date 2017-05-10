---
layout: default
comments: true
title:  "Troubleshoot SQL Server performance issues"
date:   2017-05-13 19:55:01 +0100
categories: SQL
description: "I wanted to write a reminder for myself on things to do in order to troubleshoot sql server performance issues..."
---
# [](#header-1)Troubleshoot SQL Server performance issues

I wanted to write a reminder for myself on things to do in order to troubleshoot sql server performance issues. The idea is to keep this post updated and continue to add suggestions time to time.

## [](#header-3) Troubleshoot High CPU usage

Some things that might be worth to check..

 - Check for missing indexes/stats?
 - Check for cardinality estimation problems
 - Check query plan
 - Find most expensive queries
 - Check for deadlocks
 
## [](#header-3) Find most expensive queries
 
 It's possible to use the activity monitor to find the recent most expensive queries

![sql screenshot1]({{ site.url }}/assets/sqltroubleshooting/activitymonitor.jpg)

## [](#header-3) Check for deadlocks
 
 We can use Xevents in SSMS and the xml_deadlock_report event to find deadlocks
 
 Check post on Xevents: <a href="https://maciti.github.io/sql/2017/05/01/sql-server-check-sp-recompilation-xevents.html"> XEvents post </a>

## [](#header-3) Check index fragmentation and last stats update

right click on index > properties > fragmentation

You should consider to rebuild if the fragmentation is higher than 30%
 
![sql screenshot2]({{ site.url }}/assets/sqltroubleshooting/index1.PNG)

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://maciti-github-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
  
{% endif %}
