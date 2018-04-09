---
layout: default
comments: true
title:  "Add wsse security header to soap request in WCF"
date:   2017-06-05 19:55:01 +0100
categories: wcf
description: "Add wsse security header to soap request in WCF injecting the wsse security header in the soap request before it gets submitted"
---
# [](#header-1)Add wsse security header to soap request in WCF

It could happen that you have to interface your WCF client to a web service that uses WS-Security/WSE protocol. Web Services Security (WS-Security, WSS) is an extension to SOAP to apply security to Web services. 

Unfortunately WS-Security/WSE protocol is not natively supported in .NET WCF 4.

WSS incorporates security features in the header of a SOAP message. Inside the header of a SOAP message, WSS has a security element (one or more than one) used to verify the end-user identity. Example:

{% highlight xml %}

<soap:Envelope>
 <soap:Header>	     
  <wsse:Security>
   <wsse:UsernameToken>
    <wsse:Username>user</wsse:Username>
    <wsse:Password>password</wsse:Password>
   </wsse:UsernameToken>
  </wsse:Security>

{% endhighlight %}

In order to allow your WCF client application to communicate with a WSS service a possible solution could be the following:

1. Capture the raw soap message before it gets submitted: <a href="http://mbsguru.blogspot.ie/2012/11/capturing-and-using-raw-soap-messages.html"> capture raw soap messages</a> 

2. Implement BeforeSendRequest method in CapturingMessageInspector class (the following code is going to modify the soap message injecting the wss security header):

{% highlight csharp %}

public object BeforeSendRequest(ref System.ServiceModel.Channels.Message request, IClientChannel channel)
{
    var soapModified = InjectWsseHeader(request.ToString());

    using (var memoryStream = new MemoryStream())
    using (var writerStream = new StreamWriter(memoryStream)) 
	{
        writerStream.Write(soapModified);
        writerStream.Flush();
        memoryStream.Position = 0;

        var reader = XmlReader.Create(memoryStream);
        request = System.ServiceModel.Channels.Message.CreateMessage(reader, int.MaxValue, request.Version);
    }

    this.SoapMessages.Request = soapModified;
    return null;
}

{% endhighlight %}


{% highlight csharp %}

private string InjectWsseHeader(string request)
{
    XmlDocument xmlDoc = new XmlDocument();

    xmlDoc.LoadXml(request);

    XmlNode root = xmlDoc.DocumentElement;

    var sseNamespace = "http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd";

    XmlElement sseSecurityHeader = xmlDoc.CreateElement("wsse", "Security", sseNamespace);

    XmlElement usernameToken = xmlDoc.CreateElement("wsse", "UsernameToken", sseNamespace);

    XmlElement username = xmlDoc.CreateElement("wsse", "Username", sseNamespace);

    username.InnerText = "your username ..";

    XmlElement password = xmlDoc.CreateElement("wsse", "Password", sseNamespace);

    password.InnerText = "your password ..";

    usernameToken.AppendChild(username);

    usernameToken.AppendChild(password);

    sseSecurityHeader.AppendChild(usernameToken);

    root.FirstChild.AppendChild(sseSecurityHeader);

    using (var stringWriter = new StringWriter())
    using (var xmlTextWriter = XmlWriter.Create(stringWriter))
    {
        xmlDoc.WriteTo(xmlTextWriter);
        xmlTextWriter.Flush();
        return stringWriter.GetStringBuilder().ToString();
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
this.page.url = 'https://maciti.github.io/wcf/2017/06/05/add-wsse-security-header-to-soap-request-in-WCF.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2017-06-05-add-wsse-security-header-to-soap-request-wcf'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
