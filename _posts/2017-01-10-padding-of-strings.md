---
ID: 15
post_title: Padding of strings
author: Christian Gräfe
post_date: 2017-01-10 10:58:07
post_excerpt: ""
layout: post
permalink: >
  http://passionindata.de/2017/01/10/padding-of-strings/
published: true
mytory_md_visits_count:
  - "1"
---
<h1>Preamble</h1> <p>With this post I will start blogging about SQL Server, Powershell &amp; C#. Starting with a T-SQL Tuesday blog post, which is hosted this month by <a href="https://www.brentozar.com/archive/2017/01/announcing-t-sql-tuesday-87-sql-server-bugs-enhancement-requests/">Brent Ozar</a>. </p> <p align="right">&nbsp; <img src="https://www.brentozar.com/wp-content/uploads/2016/11/tsql2sday150x150.jpg"></p> <h1>Introduction</h1> <p>A long time ago I filed a <a href="https://connect.microsoft.com/SQLServer/Feedback/Details/728597">connect item</a> to request new funtions for padding a value with a defined value. I used and still need such a feature to prepare data for export data to system like MVS.</p> <p>Example from Oracle with the left padding function</p> <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:566d0ba3-970e-4046-b030-12d8cef4ebda" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px"><pre style=white-space:normal>
<pre><code class="sql">
[sourcecode language='sql'  padlinenumbers='true' htmlscript='false' collapse='false']
select lpad(10, 4, ‘0’) padd from dual;
</code></pre>
</pre>
</div>
<p>PADD<br>----<br>0010</p>
<p>&nbsp;</p>
<h1>Padding by T-SQL</h1>
<p>Padding via T-SQL I realized via a function like this:</p>
<p>&nbsp;</p>
<div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:9ab7df1f-338d-447f-8294-a6b3e97c8720" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px"><pre style=white-space:normal>
<pre><code class="sql">
CREATE FUNCTION [dbo].[test_TSQL_LPAD_REPLICATE] (@nWert as int, @nLaenge as tinyint )
RETURNS varchar(20)
AS
BEGIN

	RETURN(replicate(&#39;0&#39;, @nLaenge - len(cast(@nWert as varchar(20)))) + cast(@nWert as varchar(20)))
END
</code></pre>
</pre>
</div>
<p>Running this function over&nbsp; table with about 1.3m rows takes 31 sec. </p>
<h1>Padding by SQLCLR</h1>
<p>Padding via SQLCLR I relized with a database project with the following C# code:</p>
<div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:6c9c3d0c-7b5b-4fc7-a346-d28f6bbc3222" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px"><pre style=white-space:normal>
<pre><code class="csharp">
using System.Data.SqlTypes;

public partial class UserDefinedFunctions
{
    [Microsoft.SqlServer.Server.SqlFunction(IsDeterministic = true)]
    public static SqlString clr_LPAD(SqlInt32 myValue, SqlInt32 length)
    {
        // Put your code here
        return myValue.ToString().PadLeft((System.Int32)length, &#39;0&#39;);
    }
}
</code></pre>
</pre>
</div>
<p>May there ist a better way in C# to implement this, but this just works. <img class="wlEmoticon wlEmoticon-smile" style="border-top-style: none; border-bottom-style: none; border-right-style: none; border-left-style: none" alt="Smile" src="http://sharptools.de/wp-content/uploads/2017/01/wlEmoticon-smile.png"> Running the same 1.3m rows takes 35 sec. So there is no difference compared to the T-SQL implementation</p>
<h1>Summary</h1>
<p>At the end of the day, I want a standard function without implementing this common helper functionality at every database. Additionally I assume a native implementation of left or right padding will perform much better as my custom functions.</p>
<p>So, if you agree with me, please vote for my <a href="https://connect.microsoft.com/SQLServer/Feedback/Details/728597">connect item #728597</a></p>