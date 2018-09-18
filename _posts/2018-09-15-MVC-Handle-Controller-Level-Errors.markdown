---
layout: default
comments: true
title:  "MVC Handle Controller-Level Errors"
date:   2018-09-15 19:55:01 +0100
categories: MVC
description: "a way to handle controller-level errors"
---
# [](#header-1) MVC 5 How to Handle Controller-Level Errors

Catching and handling errors at controller level can be very useful. Sometimes we don't want to handle some errors at application level <a href=https://msdn.microsoft.com/en-us/library/24395wz3.aspx">How to: Handle Application-Level Errors</a>.

There are different ways to handle controller-level errors in MVC. Each one of them with its own pro and cons.

For example you can use an exception filter or you can override the OnException method of the controller class. I'm going to show the latter. 

I just created a new controller named "TestController", and a new View named "Error" under the View > Test folder. 

{% highlight csharp %}

    public class TestController : Controller
    {
        public ActionResult Index()
        { 
           throw new NotImplementedException();
        }

        protected override void OnException(ExceptionContext exceptionContext)
        {
            //retrieve the exception
            Exception ex = exceptionContext.Exception;
            exceptionContext.ExceptionHandled = true;

            //retrieve other info
            var controllerName = exceptionContext.RouteData.Values["controller"] as string;
            var actionName = exceptionContext.RouteData.Values["action"] as string;
            var errorInfo = new HandleErrorInfo(exceptionContext.Exception, controllerName, actionName);

            //DO SOME LOGGING

            //redirect to error view
            exceptionContext.Result = new ViewResult { ViewName = "Error" };
        }
    }

{% endhighlight %}

The exception thrown in the action Index will be caught by the OnExection method. In the exceptionContext object you will have all the info you need for logging the error for example. 

You could also return specific views based on the type of the exception caught. For example a "NotFound" view if a custom ProductNotFoundException is thrown by the action Details(string sku) of the ProductController.

NOTE: You can't handle controller-level errors in this way using .NET core. There isn't a OnException method in the controller class anymore.

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/mvc/2018/09/15/MVC-Handle-Controller-Level-Errors.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2018-09-15-MVC-Handle-Controller-Level-Errors'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
