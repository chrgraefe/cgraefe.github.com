alter procedure dbo.IsValidSQL (@sql varchar(max), @ErrorMessage NVARCHAR(4000) OUT) as
begin
	
    begin try
        set @sql = 'set parseonly on;'+@sql;
        exec(@sql);
    end try
    begin catch


		SET @ErrorMessage = FORMATMESSAGE('The SQL query is invalid! The check provided the following Message: " %s " at line %d', ERROR_MESSAGE(), ERROR_LINE())
		return 0;

    end catch;
    
	return 1;
end; -- IsValidSQL