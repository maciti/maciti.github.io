---
layout: default
comments: true
title:  "SqlServer check SP recompilation with XEvents"
date:   2017-05-01 19:55:01 +0100
categories: SQL
description: "I wanted to do some tests to check and monitor when SPs get recompiled, using sql_statement_recompile event.."
---
# [](#header-1)SqlServer check SP recompilation with XEvents

Recently I wanted to do some tests to check and monitor when SPs get recompiled (SP recompilation can be expensive, especially in terms of CPU).

To do so I had to use sql_statement_recompile event, sql_statement_recompile event occurs when a statement-level recompilation is required by any kind of batch.

## [](#header-2) XEvents - create new session

Using Sql Server Management Studio 2016 I created a new session using the session wizard of Extended Events.

![xevents screenshot1]({{ site.url }}/assets/sqlrecompilation/sessionwizard.jpg)

![xevents screenshot2]({{ site.url }}/assets/sqlrecompilation/xevents1.png)

I didn't want a pre-built template..

![xevents screenshot3]({{ site.url }}/assets/sqlrecompilation/xevents2.png)

I added the sql_statement_recompile event..

![xevents screenshot4]({{ site.url }}/assets/sqlrecompilation/xevents3.png)

Here you can see that the wizard states that "sql_statement_recompile occurs when a statement-level recompilation is required by any kind of batch. This includes stored procedures, triggers, ad hoc batches and queries. Batches may be submitted through several interfaces, including sp_executesql, dynamic SQL, Prepare methods or Execute methods"

..and using the next form all the global params I wanted to capture

![xevents screenshot5]({{ site.url }}/assets/sqlrecompilation/xevents4.png)

I skipped all the remaining forms (I didn't need filters and I didn't want to save the data locally)

## [](#header-3) Watch Live Data

Right click on the session just created, click on "Start Session" and then click on "Watch Live Data"

![xevents screenshot6]({{ site.url }}/assets/sqlrecompilation/startsession.jpg)

## [](#header-3) Some Tests

In order to test it I created some stored procedures using AdventureWorks2014 Database.

I knew that the use of temp table in a SP could cause a recompilation in order to create an optimal execution plan.

So I created a simple SP

{% highlight sql %}

USE [AdventureWorks2014]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:		<Author,,Name>
-- Create date: <Create Date,,>
-- Description:	<Description,,>
-- =============================================
CREATE PROCEDURE [dbo].[TestRecompile1] 
AS
BEGIN

	SET NOCOUNT ON;

    CREATE TABLE #Temp (ID int PRIMARY KEY, FirstName nvarchar(50), LastName nvarchar(50))

	INSERT INTO #Temp select top 100 BusinessEntityID, FirstName, LastName from Person.Person

	DROP TABLE #Temp  

END

{% endhighlight %}

Running the SP I can see the recompile event as expected

![xevents screenshot8]({{ site.url }}/assets/sqlrecompilation/recompile1.PNG)

Another test that caused recompilation was the modification of the clustered index.

I also tried to replace the temp table with a table variable, as expected the recompilation event wasn't fired. Table variable is lighter, faster, but with no statistics so with cardinality estimation problems. They are ok for tables with few number of rows but not good when the table is big.

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://maciti-github-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
  
{% endif %}
