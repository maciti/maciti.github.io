---
layout: default
title:  "Mock Service in SoapUi"
date:   2017-04-23 19:55:01 +0100
categories: SOA
---
# [](#mock service)Mock Service in SoapUI

SoapUI provides a feature to create mock services. Suppose you have to integrate your application with a third party service based on a WSDL. Maybe, for some reasons, you don’t want to call the real service,  or the real service is still under development. 

SoapUI, with the mock service feature, allows you to create a fake service that simulates the real one.

## [](#example)Simple Example

Let’s suppose we have to call a web service dedicated to retrieve stock item information such as the availability of a product in the storehouse and, unfortunately, the service is still under development.

{% highlight ruby %}

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
