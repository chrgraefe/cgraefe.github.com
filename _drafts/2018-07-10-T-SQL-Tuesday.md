---
ID: 114
post_title: T-SQL Tuesday
author: sharptools_wp
post_excerpt: ""
layout: post
permalink: http://passionindata.de/?p=114
published: false
---
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