### ToDo links
https://www.youtube.com/watch?v=SnSgl2JfG9U

https://rarus.ru/publications/20201009-ot-ekspertov-1c-rarus-jenkins-1c-446784/#vtoroy-sposob-pipeline

### Other notes
http://ukveterperemen.ru/
[info@ukveterperemen.ru](mailto:info@ukveterperemen.ru)


http://10.78.77.62:9000/dashboard?id=EK2017-post_refactoring
http://ts.nostrum.ru:3000/projects/ns-063/issues

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


![[Pasted image 20211110233735.png]]





	ТаблицаДанных = Новый ТаблицаЗначений:
	ТаблицаДанных.Колонки.Добавить("Профиль");
	ТаблицаДанных.Колонки.Добавить("ИмяПользователя");
	ТаблицаДанных.Колонки.Добавить("ПользовательОС");
	
	ТекстВводныхДанных = Новый ТекстовыйДокумент;
	ТекстВводныхДанных.Прочитать(ПутьКФайлу);
	
	ВсегоСтрок = ТекстВводныхДанных.КоличествоСтрок();
	
	Для НомерСтроки = 1 По ВсегоСтрок Цикл
		
		ТекстСтрокаДанных = ТекстВводныхДанных.ПолучитьСтроку(НомерСтроки);
		РазложитьСтрокуВМассив(ТекстСтрокаДанных)	
		НоваяСтрока = ТаблицаДанных.Добавить();
		НоваяСтрока.		
			
	КонецЦикла;


### Разное
https://auto.ru/cars/used/sale/mazda/cx_5/1105712767-b303510f/