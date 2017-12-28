---
post_title: How to consider trailing spaces
layout: post
permalink:
published: false
---

Sometimes it is neccessary to take any character into account. The standard behavior of sql SQL Server LEN function ignores trailing spaces.

so if you try this
<pre><code class="powershell">
	SELECT LEN(' 234 ') length
</code></pre>
it returns 4


If you need all characters, even the trailing spaces. You have to replace them BEFORE you get the length via LEN funtion.
<pre><code class="sql">
DECLARE
 @VARCHAR6 VARCHAR(6) = ' 1234 '

select len(@VARCHAR6) _Len,  len(REPLACE(@VARCHAR6, ' ', '#')) _LenWithSpaceReplace,  datalength(@VARCHAR6) _DataLength
</code></pre>

I hope it helps.