---
layout: default
comments: true
title:  "The ExpandoObject Class"
date:   2019-12-30 19:55:01 +0100
categories: C#
description: "Some simple code to start using the ExpandoObject class"
---
# [](#header-1) The ExpandoObject Class

The class ExpandoObject from System.Dynamics represents an object whose members can be dynamically added and removed at run time.
The ExpandoObject supports runtime binding and implements the DLR interface IDynamicMetaObjectProvider, allowing you to share the expandoobject written in c# with another language that supports DLR interop.

The structure of ExpandoObject is a Dictionary of type string, object...as you can see in the reference source <a href="https://github.com/microsoft/referencesource/blob/master/System.Core/Microsoft/Scripting/Actions/ExpandoObject.cs">ExpandoObject class</a>.

I created a simple object with some attributes and methods:

{% highlight csharp %}
            
dynamic person = new ExpandoObject();

person.Name = "John";
person.Surname = "Lastname";
person.DateOfBirth = "30-Dec-1960";
person.Mood = "happy";

person.PresentYourself = new Action(() => Console.WriteLine($"Hello! My name is {person.Name}"));
person.ChangeMood = new Action<string>(x => person.Mood = x);

{% endhighlight %}

Being a IDictionary<string, object> type, it allows me to print all the properties in the same way I'd do for a dictionary

{% highlight csharp %}

foreach (var property in person)
{
    Console.WriteLine(property.Key + ": " + property.Value);
}

{% endhighlight %}

output:
> Name: John
> Surname: Lastname
> DateOfBirth: 30-Dec-1960
> Mood: happy
> PresentYourself: System.Action
> ChangeMood: System.Action`1[System.String]

Thanks to dynamic binding I'm able to access the properties of my ExpandoObject in a simple way e.g. person.Mood...without the need of using GetProperty() GetAttribute() etc

{% highlight csharp %}

person.ChangeMood("sad");

Console.WriteLine(person.Mood);

person.PresentYourself();

{% endhighlight %}

output:
> sad
> Hello! My name is John

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/c%23/2019/12/30/The-ExpandoObject-Class.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2019-12-30-The-ExpandoObject-Class'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
