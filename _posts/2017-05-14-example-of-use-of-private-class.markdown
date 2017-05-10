---
layout: default
comments: true
title:  "A simple example of use of a private class in C#"
date:   2017-05-14 19:55:01 +0100
categories: c-sharp
description: "A simple example of use of a private class in C#"
---
# [](#header-1)Private class example C#

A simple example of use of a private class in C#

{% highlight csharp %}

    public class Car
    {
        private Engine _engine { get; set; }

        public Car()
        {
            _engine = new Engine();
        }

        public void TurnStartKey()
        {
            Console.WriteLine("lights on..");

            _engine.Start();
        }

        private class Engine
        { 
            public void Start()
            {
                Console.WriteLine("engine started");
            }

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
this.page.url = 'https://maciti.github.io/c-sharp/2017/05/14/example-of-use-of-private-class.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2017-05-14-example-of-use-of-private-class'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
