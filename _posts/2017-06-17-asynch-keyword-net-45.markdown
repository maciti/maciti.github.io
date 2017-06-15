---
layout: default
comments: true
title:  "Asynch keywork in .NET 4.5"
date:   2017-06-17 19:55:01 +0100
categories: c-sharp
description: "Since .net 4.5 the CLR can call any method asynchronously in an automatic way"
---
# [](#header-1) Asynch keywork in .NET 4.5

Since .net 4.5 the CLR can call any method asynchronously in an automatic way, this is a simple example to show how to use it 

In the following I'm trying to simulate a real-life example where a job has to complete a task asynchronously, like sending an email, but has to continue with his execution while the process of sending the email is running. 

{% highlight csharp %}

    class Program
    {
        static void Main(string[] args)
        {
            Task t = SendEmailAsynch();

            Console.WriteLine("Doing other stuff");

            Console.ReadLine();

        }

        public static async Task SendEmailAsynch()
        {
            Console.WriteLine("asynch email task..");

            await myTask();
        }

        private static Task myTask()
        {
            var t = new Task(() => {

                Console.WriteLine("sending..");
                Thread.Sleep(3000);
                Console.WriteLine("send completed");
            });

            t.Start();

            return t;
        }
    }

{% endhighlight %}

The execution of it is going to print the following:

![asynch screenshot]({{ site.url }}/assets/Asynch/asynchResult.PNG)

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/c-sharp/2017/06/17/asynch-keyword-net-45.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2017-06-17-asynch-keyword-net-45'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
