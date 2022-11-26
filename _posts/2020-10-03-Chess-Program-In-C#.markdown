---
layout: default
comments: true
title:  "Chess Program in C#"
date:   2020-10-03 19:55:01 +0100
categories: C#
description: "A chess program written in win form net core 3.1 with AI"
---
# [](#header-1) Chess Program in C#

![chess screenshot]({{ site.url }}/assets/ChessAI/board.PNG)

I've created a simple project to play chess against the computer. 

The AI algorithm is a minimax implementation with alpha-beta pruning.

More info and the source code can be found here <a href="https://github.com/maciti/ChessAI">https://github.com/maciti/ChessAI</a>

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/c%23/2020/10/03/Chess-Program-In-C%23.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2020-10-03-Chess-Program-In-C%23'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
