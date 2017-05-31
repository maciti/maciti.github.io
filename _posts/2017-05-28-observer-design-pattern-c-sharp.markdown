---
layout: default
comments: true
title:  "An example of use of the observer design pattern in c sharp with events and delegates"
date:   2017-05-20 19:55:01 +0100
categories: c-sharp
description: "This is my implementation of the observer design pattern using c#"
---
# [](#header-1)An example of use of the observer design pattern in c sharp

This is my implementation of the observer design pattern using c#. I created a simple example using an event and a delegate. 

The TicketSeller object (that has the purpose to sell tickets) has an event named OnSale. You can subscribe to this event in order to be notified when the tickets are on sale. 

{% highlight csharp %}

     public delegate void SaleStatusHandler();

    public enum SaleStatus
    {
        Closed,
        Announced,
        OnSale,
        Expired
    }

    public class TicketSeller
    {
        public event SaleStatusHandler OnSale;

        private SaleStatus _saleStatus;
        public SaleStatus SaleStatus
        {
            get {

                return _saleStatus;
            }

            set
            {
                _saleStatus = value;

                if (_saleStatus == SaleStatus.OnSale && OnSale != null)
                    OnSale();
            }

        }
    }

{% endhighlight %}


How to subscribe? (lambda syntax)

{% highlight csharp %}

	TicketSeller t = new TicketSeller();

        //lambda subscription
    t.OnSale += () => Console.WriteLine("Tickets are on sale now");

{% endhighlight %}


or using other different syntaxes

{% highlight csharp %}

    //classic subscription
    t.OnSale += TicketsAreOnSale;

    //or..
    t.OnSale += new SaleStatusHandler(TicketsAreOnSale);
			
			
	public static void TicketsAreOnSale()
	{
		Console.WriteLine("------ tickets are on sale ------");
	}

{% endhighlight %}


How to test it? As soon as the status of the tickets goes on sale you will be notified..

{% highlight csharp %}

	t.SaleStatus = SaleStatus.OnSale;
	
	Console.ReadLine();
			
{% endhighlight %}


{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/c-sharp/2017/05/28/observer-design-pattern-c-sharp.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2017-05-20-observer-design-pattern-c-sharp'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
