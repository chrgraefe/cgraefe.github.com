DECLARE
 @VARCHAR6 VARCHAR(6) = ' 1234 '
,@NVARCHAR6 NVARCHAR(6) = N' 1234 '

select 'VARCHAR'  DataType, len(@VARCHAR6) _Len,  len(REPLACE(@VARCHAR6, ' ', '#')) _LenWithSpaceReplace,  datalength(@VARCHAR6) _DataLength UNION
select 'NVARCHAR' DataType, len(@NVARCHAR6) _Len, len(REPLACE(@NVARCHAR6, ' ', '#')) _LenWithSpaceReplace, datalength(@NVARCHAR6) _DataLength 

