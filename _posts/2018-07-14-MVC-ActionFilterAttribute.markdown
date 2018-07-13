---
layout: default
comments: true
title:  "MVC ActionFilterAttribute"
date:   2018-07-14 19:55:01 +0100
categories: MVC
description: "MVC ActionFilters and ActionFilterAttribute"
---
# [](#header-1) ActionFilterAttribute. 

ActionFilterAttribute is an abstract class upon which action filters are based. It allows the creation of custom action filters.

There are 4 methods that a custom action filter that implements ActionFilterAttribute can override:

- OnActionExecuting
- OnActionExecuted
- OnResultExecuting
- OnResultExecuted 

## [](#header-3) Example of custom action filter

I created a simple custom action filter. The purpose of this filter is to clear a portion or the entire content of the cache once a particular controller action is hit. 

{% highlight csharp %}

    public class ClearCache : ActionFilterAttribute
    {
        public bool ClearAll { get; set; }

        public string Key { get; set; }

        public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            var cacheProvider = new SimpleCacheProvider();

            if (ClearAll)
            {
                cacheProvider.ClearAll();
            }

            if (!String.IsNullOrEmpty(Key))
            {
                cacheProvider.Clear(Key);
            }
        }
    }

{% endhighlight %}


I added 2 properties, ClearAll to clear the entire content of the cache and Key, to clear a specific key/value from the cache.

This is the simple implementation that I wrote for the cache provider:

{% highlight csharp %}

    public interface ICacheProvider
    {
        bool Get<T>(string key, out T value);

        void Set<T>(string key, T value);

        void Set<T>(string key, T value, int duration);

        void Clear(string key);

        void ClearAll();
    }
	
	public class SimpleCacheProvider : ICacheProvider
    {
        private readonly int defaultCacheDurationInMinutes = 30;

        private Cache cache;

        public SimpleCacheProvider()
        {
            cache = HttpRuntime.Cache;
        }

        public void Clear(string key)
        {
            cache.Remove(key);
        }

        public void ClearAll()
        {
            List<string> keys = new List<string>();

            foreach (DictionaryEntry item in cache)
            {
                keys.Add(item.Key as string);
            }

            keys.ForEach(x => cache.Remove(x));
        }

        public bool Get<T>(string key, out T value)
        {
            try
            {
                if (cache[key] == null)
                {
                    value = default(T);
                    return false;
                }

                value = (T)cache[key];
            }
            catch
            {
                value = default(T);
                return false;
            }

            return true;
        }

        public void Set<T>(string key, T value)
        {
            Set<T>(key, value, defaultCacheDurationInMinutes);
        }

        public void Set<T>(string key, T value, int duration)
        {
            cache.Insert(key,value,null, DateTime.Now.AddMinutes(duration), TimeSpan.Zero);
        }
    }

{% endhighlight %}

## [](#header-3) Use the attribute on controller action

{% highlight csharp %}

        [ClearCache(ClearAll = true)]
        public ActionResult TestActionX()
        {
            return View();
        }
		
		[ClearCache(Key = "test")]
        public ActionResult TestActionY()
        {
            return View();
        }
		

{% endhighlight %} 

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/mvc/2018/07/13/MVC-ActionFilterAttribute.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2018-07-13-MVC ActionFilterAttribute'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
