---
layout: default
comments: true
title:  "WCF client, sending multiple requests in parallel to a web service"
date:   2017-07-13 19:55:01 +0100
categories: WCF
description: "A simple example of sending multiple requests (in parallel) to a web service, using WCF client and Task class"
---
# [](#header-1) WCF client, sending multiple requests in parallel

A simple example of sending multiple requests (in parallel) to a web service, using WCF client and Task class.

{% highlight csharp %}

    List<Request> requests = new List<Request>();

    //represents the batch size of requests sent in parallel
    int degreeOfParallelism = 3;

    using (var client = HttpClientFactory.Create())
    {
        client.Open();

        int i = 0;

        while (i < requests.Count) {

            //list of task to be run in parallel
            var tasks = new List<Task<Response>>();

            for(int y = 0; i < degreeOfParallelism; y++){

                int position = i + y;

                if (position >= requests.Count)
                    break;

                //adding task to task list
                tasks.Add(new Task<Response>(() => { return client.Send(requests[position]); }));
            }

            //starting all tasks
            tasks.ForEach(x => x.Start());

            //waiting all tasks to be completed
            Task.WaitAll(tasks.ToArray());

            //collecting/printing the responses received 
            tasks.ForEach(x => Console.WriteLine(x.Result));

            i += degreeOfParallelism;
        }
    }

{% endhighlight %}


{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/wcf/2017/07/13/WCF-client-send-multiple-requests-parallel.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2017-07-13-WCF-client-send-multiple-requests-parallel'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
