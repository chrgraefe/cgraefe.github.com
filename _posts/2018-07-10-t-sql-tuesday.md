---
ID: 118
post_title: T-SQL Tuesday
author: sharptools_wp
post_excerpt: ""
layout: post
permalink: >
  http://passionindata.de/2018/07/10/t-sql-tuesday/
published: true
post_date: 2018-07-10 15:52:10
---
# Code I can't live without

This is my contribution to the TSQL-Tuseday #104 ![TSQL Tuesday][1]

## "Left" padding of numbers

One of the things I wouldn't miss in my SQL Server environment, is my dbo.padl function. Like the lpad function on oracle, this function is padding "0"'s to the left for a given number.

*Example*

<pre class="sql">SELECT dbo.PADL(12, 3)

Result: 012
</pre>

The code is derived from several forum or blog posts from the early 2000er days. There may be exists more efficient ways to do the same, but this worked for since years.

*Code*

<pre class="sql">CREATE FUNCTION [dbo].[PADL]
(
  @nValue DECIMAL
, @nLength TINYINT
)
RETURNS VARCHAR(MAX)
AS
BEGIN

    DECLARE @cValue VARCHAR(20)
    SET @cValue = CONVERT(VARCHAR(50), @nValue)

    SET @cValue = REPLICATE('0', @nLength - LEN(LTRIM(RTRIM(CONVERT( VARCHAR(MAX), @nValue))))) + LTRIM(RTRIM(CONVERT( VARCHAR(MAX), @nValue)))

RETURN @cValue
END
</pre>

## Do I really need it?

I often discuss this code and several other helper functions for string formatting with colleagues. We never found a good replacement for this functionality, so it stays in my tool belt.

May be Microsoft will implement a similar function in SQL Server like Oracle has for years. You can upvote my [feature request][2]



 [1]: http://tsqltuesday.azurewebsites.net/wp-content/uploads/2017/02/tsqltuesday.jpg "T-SQL Tuesday"
 [2]: https://feedback.azure.com/forums/908035-sql-server/suggestions/32896552-provide-lpad-rpad-funtion