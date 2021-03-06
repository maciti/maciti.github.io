---
layout: default
comments: true
title:  "Mock Service in SoapUi"
date:   2017-04-25 19:55:01 +0100
categories: SOA
description: "SoapUI provides a feature to create mock services. Suppose you have to integrate your application with a third party service based on a WSDL. Maybe, for some reasons, you don’t want to call the real service,  or the real service is still under development..."
---
# [](#header-1)Mock Service in SoapUI

SoapUI provides a feature to create mock services. Suppose you have to integrate your application with a third party service based on a WSDL. Maybe, for some reasons, you don’t want to call the real service,  or the real service is still under development. 

SoapUI, with the mock service feature, allows you to create a fake service that simulates the real one.

## [](#header-3) Web Service

Let’s suppose we have to call a web service dedicated to retrieve stock item information such as the availability of a product in the storehouse and, unfortunately, the service is still under development.

{% highlight csharp %}

[WebService(Namespace = "http://tempuri.org/")]
[WebServiceBinding(ConformsTo = WsiProfiles.BasicProfile1_1)]
public class StockItemService : System.Web.Services.WebService {

    public StockItemService () { }

    [WebMethod]
    public AvailabilityInfo GetAvailability(string itemCode) {

        throw new NotImplementedException();
    }  
}

{% endhighlight %}

## [](#header-3) new SoapUI project

So we retrieve or we get provided with the WSDL. 
In SoapUI we create a new SoapUI project importing the WSDL.
 
![soapui project screenshot]({{ site.url }}/assets/soapui/SoapUI1.png)

Then SoapUI will create automatically the interfaces  for the real service.

![soapui project screenshot 2]({{ site.url }}/assets/soapui/SoapUI2.png)

## [](#header-3) Generate mock service

Now, in order to create a mock service,right click on the interface and click on “generate mock service”

![mock service screenshot]({{ site.url }}/assets/soapui/SoapUI3.png)

Now, we can create the responses for the specific service methods:

![mock response screenshot]({{ site.url }}/assets/soapui/soapUI5.png)

![mock response screenshot 2]({{ site.url }}/assets/soapui/soapUI6.png)

## [](#header-3) Route the request to the desired response

The last thing we need to do is to dinamically rout the request to the desired response.

We can select a response based on the request content.  In order to achieve this we could create a simple script.

Double click on the service method, as a dispatch method we select “script”. With an XPath query we retrieve the itemCode and based on that  value we return the desired response.

To run the script click on green arrow 

![My helpful script screenshot]({{ site.url }}/assets/soapui/soapui7.png)

{% highlight ruby %}

import groovy.util.XmlSlurper

def nodesResult = mockRequest.getContentElement().selectPath("//*[contains(local-name(), 'itemCode')]")

def itemCode = nodesResult[0].getStringValue()

log.info itemCode

if(itemCode == "000001")
	return "available"
else if(itemCode == "000002")
	return "out of stock"


{% endhighlight %}

## [](#header-3) Test it!

Start the mock service: double click on the service and click on the start arrow

Submit a request:  right click on the service method, click “open request”, modify your imput parameters and click on the submit arrow.


{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/soa/2017/04/25/mock-service-in-soapui.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2017-04-25-mock-service-in-soapui'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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

