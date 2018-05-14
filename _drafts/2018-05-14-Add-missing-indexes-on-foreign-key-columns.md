---
ID: 110
post_title: >
  Add missing indexes on foreign key
  columns
author: sharptools_wp
post_excerpt: ""
layout: post
permalink: http://passionindata.de/?p=110
published: false
---
# Add indexes quick and easy

Inspired by the entry "[Unindexed Foreign Keys][1]" from Brent's [Weekly Links][2], I extended the provided script with ready to deploy sql code. Have fun!

<pre><code class="sql">
;WITH fk_cte AS 
( 
    SELECT   
    SCHEMA_NAME(soPkTable.schema_id) PK_TABLE_SCHEMA_NAME,
    OBJECT_NAME(fk.referenced_object_id) PK_TABLE,
    c2.name PK_COLUMN,
    kc.name PK_INDEX_NAME,
    SCHEMA_NAME(soFkTable.schema_id) FK_TABLE_SCHEMA_NAME,
    OBJECT_NAME(fk.parent_object_id) FK_TABLE,
    c.name FK_COLUMN,
    fk.name FK_NAME,
    fk.parent_object_id
    FROM sys.foreign_keys fk
    INNER JOIN sys.foreign_key_columns fkc 
        ON fkc.constraint_object_id = fk.object_id
    INNER JOIN sys.objects soPkTable
        ON soPkTable.object_id = fk.referenced_object_id
    INNER JOIN sys.objects soFkTable
        ON soFkTable.object_id = fk.parent_object_id
    INNER JOIN sys.columns c 
        ON c.object_id = fk.parent_object_id 
        AND c.column_id = fkc.parent_column_id
    LEFT JOIN sys.columns c2 
        ON c2.object_id = fk.referenced_object_id 
        AND c2.column_id = fkc.referenced_column_id
    LEFT JOIN sys.key_constraints kc 
        ON kc.parent_object_id = fk.referenced_object_id 
        AND kc.type = 'PK'
    LEFT JOIN sys.index_columns ic 
        ON ic.object_id = c.object_id AND ic.column_id = c.column_id
    LEFT JOIN sys.indexes i 
        ON i.object_id = ic.object_id 
        AND i.index_id = ic.index_id
    WHERE
        i.object_id IS NULL
)
SELECT  
 cte.*
,'CREATE NONCLUSTERED INDEX [IX_'+cte.FK_TABLE_SCHEMA_NAME+'_'+cte.FK_TABLE+'_'+cte.FK_COLUMN+'] ON ['+cte.FK_TABLE_SCHEMA_NAME+'].['+cte.FK_TABLE+'] (['+cte.FK_COLUMN+'])' sqlForeignKey
FROM fk_cte cte
LEFT JOIN sys.dm_db_partition_stats ps 
    on ps.object_id = cte.parent_object_id 
    and ps.index_id &lt;= 1
ORDER BY used_page_count desc

    

</code>
</pre>

 [1]: https://sqlpal.blogspot.de/2018/05/dmv-to-list-of-foreign-keys-with-no.html
 [2]: https://mailchi.mp/brentozar/ffwu8288fq-1363945?e=67bb586735