---
layout: post
comments: true
title:  "Bare Metal Kubernetes Cluster"
date:   2024-01-18 19:55:01 +0100
categories: Kubernetes
description: "I'll be detailing the construction of my 3-node Kubernetes cluster on older hardware within my home lab"
---
# [](#header-1) Bare Metal Kubernetes Cluster

In this blog post, I'll be detailing the construction of my 3-node Kubernetes cluster on older hardware within my home lab. Come along as I walk you through the process and share the lessons learned in my Kubernetes learning adventure! 




{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/kubernetes/2024/01/18/bare-metal-kubernetes-cluster.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2024-01-18-bare-metal-kubernetes-cluster'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
