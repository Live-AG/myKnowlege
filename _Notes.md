hyperion
setellite
helios

### Сервер RAS для Windows

https://infostart.ru/1c/articles/810752/#p4

23ab5d4d0b9cbc1b3ae53c9fa350e9f8


#SQL
#### Размер таблиц 1С в базе данных MS SQL

	SELECT 
		t.NAME AS TableName, 
		s.Name AS SchemaName, 
		p.rows AS RowCounts, 
		SUM(a.total_pages) * 8 AS TotalSpaceKB, 
		SUM(a.used_pages) * 8 AS UsedSpaceKB, 
		(SUM(a.total_pages) - SUM(a.used_pages)) * 8 AS UnusedSpaceKB 
	FROM 
		sys.tables t 
	INNER JOIN 
		sys.indexes i ON t.OBJECT_ID = i.object_id 
	INNER JOIN 
		sys.partitions p ON i.object_id = p.OBJECT_ID AND i.index_id = p.index_id 
	INNER JOIN 
		sys.allocation_units a ON p.partition_id = a.container_id 
	LEFT OUTER JOIN 
		sys.schemas s ON t.schema_id = s.schema_id 
	WHERE 
		t.NAME NOT LIKE 'dt%' AND t.is_ms_shipped = 0 AND i.OBJECT_ID > 255 
	GROUP BY 
		t.Name, s.Name, p.Rows ORDER BY t.Name
		

		
```
http://AGusarov:gFKrZ6G9LWJXAxQ2ubcu@localhost:9999/root/myObsidian
```

### Обработки свертки баз
https://infostart.ru/public/1242480/
https://infostart.ru/public/1206381/
https://infostart.ru/public/153571/
https://infostart.ru/public/1154357/


# Модульная (open source) конфигурация "INFOSTART ERP community edition"
https://infostart.ru/public/1285144/

### Читать

https://infostart.ru/1c/articles/703384/
https://infostart.ru/1c/articles/811452/