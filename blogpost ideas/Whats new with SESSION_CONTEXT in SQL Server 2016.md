DECLARE 
 @largeValue VARCHAR(8000) =  REPLICATE('#', 8000)
,@largeValueWS NVARCHAR(4000) =  REPLICATE(N'#', 4000)

 -- max 8000 Bytes = 8000 chars VARCHAR = 4000 chars NVARCHAR
 -- @read_only = 1 (only readable)
 -- @read_only = 0 (read/writeable) is default
EXEC sp_set_session_context @key = N'language', @value = 'English';  
EXEC sp_set_session_context @key = N'language_code', @value = 'en_US';  
EXEC sp_set_session_context @key = N'OwnOrganizationId', @value = 4711;  
EXEC sp_set_session_context @key = N'OwnLocation', @value = '0815' /*, @read_only = 1 */ ;  
EXEC sp_set_session_context @key = N'largeValue', @value = @largeValue;  
EXEC sp_set_session_context @key = N'largeValueWS', @value = @largeValueWS;  


SELECT 
 SESSION_CONTEXT(N'language') language
,SESSION_CONTEXT(N'language_code') language_code
,SESSION_CONTEXT(N'OwnOrganizationId') OwnOrganizationId
,SESSION_CONTEXT(N'OwnLocation') OwnLocation
,SESSION_CONTEXT(N'largeValue') largeValue
,SESSION_CONTEXT(N'largeValueWS') largeValueWS
;

SELECT * FROM sys.dm_os_memory_cache_counters WHERE type = 'CACHESTORE_SESSION_CONTEXT'; 
GO


--Free memeory used by context variables
EXEC sp_set_session_context @key = N'language', @value = NULL;  
EXEC sp_set_session_context @key = N'language_code', @value = NULL;  
EXEC sp_set_session_context @key = N'OwnOrganizationId', @value = NULL;  
EXEC sp_set_session_context @key = N'OwnLocation', @value = NULL;  
EXEC sp_set_session_context @key = N'largeValue', @value = NULL;  
EXEC sp_set_session_context @key = N'largeValueWS', @value = NULL;  
GO

SELECT * FROM sys.dm_os_memory_cache_counters WHERE type = 'CACHESTORE_SESSION_CONTEXT'; 

WAITFOR DELAY '00:00:05'

SELECT * FROM sys.dm_os_memory_cache_counters WHERE type = 'CACHESTORE_SESSION_CONTEXT'; 