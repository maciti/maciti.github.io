---
layout: default
comments: true
title:  "How to generate datatable from generic list in C# using reflection and extension methods..and vice versa"
date:   2017-05-20 19:55:01 +0100
categories: c-sharp
description: "I wanted to add an extension method to a generic list to convert it into a datatable.."
---
# [](#header-1)How to generate datatable from generic list (C sharp)

I wanted to add an extension method to a generic list to convert it into a datatable in order to be used as input param in a Stored Procedure

{% highlight csharp %}

    public static DataTable ConvertToDataTable<T>(this List<T> myList)
    {
        DataTable dt = new DataTable();

        var allProperties = typeof(T).GetProperties().ToList();

        //add all the columns
        allProperties.ForEach(x => dt.Columns.Add(x.Name));

        myList.ForEach(x =>
        {
            var row = dt.NewRow();

            var type = x.GetType();

            allProperties.ForEach(y =>
            {
                var propValue = type.GetProperty(y.Name).GetValue(x, null);

                //example: if I want to convert a boolean into a BIT
                ConversionAttr conversionType = y.GetCustomAttributes(typeof(ConversionAttr), false).FirstOrDefault() as ConversionAttr;

                row[y.Name] = (propValue != null && conversionType != null) ? Convert.ChangeType(propValue, conversionType.ConversionType) : propValue;

            });

            dt.Rows.Add(row);
        });

         return dt;
    }

{% endhighlight %}


You can use a custom attribute for conversion, let's say that for example you want to convert a bool into an int

{% highlight csharp %}


    public class ConversionAttribute : Attribute
    {
        public Type ConversionType { get; set; }

        public ConversionAttribute(Type type)
        {
            this.ConversionType = type;
        }

    }

{% endhighlight %}


Test it..

{% highlight csharp %}

List<Test> t = new List<Test> { new Test { IsValid = true, Description = "This is a test" } };

DataTable dt =  t.ConvertToDataTable<Test>();


//....

public class Test
{
    [ConversionAttr(ConversionType = typeof(int))]
    public bool IsValid { get; set; }

    public string Description { get; set; }

	//etc..
}
			
{% endhighlight %}


I also wrote this simple method that can be useful in case you want to convert a datarow (returned by a SP) into a specific object..

{% highlight csharp %}

    public static T ConvertToObject<T>(this DataRow dr)
    {
        var instance = Activator.CreateInstance<T>();

        for (int i = 0; i < dr.Table.Columns.Count; i++)
        {
            var prop = instance.GetType().GetProperty(dr.Table.Columns[i].ColumnName, BindingFlags.Public | BindingFlags.Instance | BindingFlags.IgnoreCase);

            if (prop != null)
                prop.SetValue(instance, Convert.ChangeType(dr[i], prop.PropertyType));
        }

        return instance;

    }

	//...
	
	//var test = dataRow.ConvertToObject<Test>();

{% endhighlight %}

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = 'https://maciti.github.io/c-sharp/2017/05/20/how-to-generate-datatable-from-generic-list-c-sharp.html';  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = '2017-05-20-how-to-generate-datatable-from-generic-list-c-sharp'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
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
