;WITH cteCHARS AS
(
	select 
	column_id id
	,'CHAR(' + CAST(column_id AS VARCHAR(2)) + ')' ZEICHEN
	from sys.columns
	where
	object_id = 22
)
select
STRING_AGG(cte.ZEICHEN, '+') CH
from cteCHARS cte




DECLARE
 @CHARS NVARCHAR(100) = CHAR(1)+CHAR(2)+CHAR(3)+CHAR(4)+CHAR(5)+CHAR(6)+CHAR(7)+CHAR(8)+CHAR(9)+CHAR(10)+CHAR(11)+CHAR(12)+CHAR(13)+CHAR(14)+CHAR(15)+CHAR(16)+CHAR(17)+CHAR(18)+CHAR(19)+CHAR(20)+CHAR(21)+CHAR(22)+CHAR(23)+CHAR(24)+CHAR(25)+CHAR(26)+CHAR(27)+CHAR(28)+CHAR(29)+CHAR(30)+CHAR(31)+CHAR(32)+CHAR(33)
,@TRANS NVARCHAR(100) = '.................................' 
,@STRING NVARCHAR(MAX) = '123'+CHAR(11) +CHAR(12)+'easy'+CHAR(16)+CHAR(17)+CHAR(18)+CHAR(19)+CHAR(20)+'peasy'+CHAR(29)+CHAR(30)+CHAR(31)
,@STRING2 NVARCHAR(MAX)

SELECT @STRING2 = TRANSLATE(@STRING, @CHARS, @TRANS)

SELECT 
 @STRING [BEFORE]
,@STRING2 [AFTER]