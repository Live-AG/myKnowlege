## Сервер
`lng-srv-iistest`
`10.78.77.62`

### Postgres
Дистриб: `postgresql-13.3-2-windows-x64`
Пароль: `postgres`
Порт: `5432`

### Java JDK
Дистриб: `bellsoft-jdk16.0.1+9-windows-amd64-full`

### Collaboration server
Дистриб: `1c_cs_10.0.47_windows_x86_64`

#### Настройка сервера
1. Установить Liberica JDK 16 (bellsoft-jdk16.0.1+9-windows-amd64-full)
2. Установить PostgresSQL (postgresql-13.3-2-windows-x64)
- Создать пользователя сервера: db_user (с правами CREATEDB) (Пароль: admin)
```
-- Role: db_user
-- DROP ROLE db_user;

CREATE ROLE db_user WITH
	LOGIN
	NOSUPERUSER
	INHERIT
	CREATEDB
	NOCREATEROLE
	NOREPLICATION
	ENCRYPTED PASSWORD 'SCRAM-SHA-256$4096:mFpryRTAVPV/JDkmZuQp5Q==$sk2IzApu9BUAhRzde9B8/tC1B5b6fGpyIHCwqoYzWmw=:2Ncjs2frFFpEQpZjKQQsq/ZCQgals4mqwkMI6lkS6es=';
```

- Создать базу: cs_db	
```
-- Database: cs_db
-- DROP DATABASE cs_db;

CREATE DATABASE cs_db WITH
	OWNER = db_user
	ENCODING = 'UTF8'
	LC_COLLATE = 'Russian_Russia.utf8'
	LC_CTYPE = 'Russian_Russia.utf8'
	TABLESPACE = pg_default
	CONNECTION LIMIT = -1;
```

- Для базы выполнить на SQL:
`CREATE EXTENSION IF NOT EXISTS "uuid-ossp";`

3. Установить Сервер взаимодействия (1ce-installer от администратора)
4. Выполнить в CMD (ПОД АДМИНИСТРАТОРОМ):
```
ring hazelcast instance create --dir C:\hazelcast
ring elasticsearch instance create --dir C:\elasticsearch
ring cs instance create --dir C:\cs

ring cs --instance cs websocket set-params --hostname localhost (реальный IP)
ring cs --instance cs websocket set-params --port 9094

ring cs --instance cs jdbc pools --name common set-params --url jdbc:postgresql://localhost:5432/cs_db?currentSchema=public
ring cs --instance cs jdbc pools --name common set-params --username db_user
ring cs --instance cs jdbc pools --name common set-params --password postgres

ring cs --instance cs jdbc pools --name privileged set-params --url jdbc:postgresql://localhost:5432/cs_db?currentSchema=public
ring cs --instance cs jdbc pools --name privileged set-params --username db_user
ring cs --instance cs jdbc pools --name privileged set-params --password postgres

ring hazelcast --instance hazelcast service create
ring elasticsearch --instance elasticsearch service create
ring cs --instance cs service create
```

5. Проверить работу сервера: http://localhost:8087/rs/health
6. Выполнить настройку
```
curl -Sf -X POST -H "Content-Type: application/json" -d "{ \"url\" : \"jdbc:postgresql://localhost(реальный IP):5432/cs_db\", \"username\" : \"db_user\", \"password\" : \"postgres\", \"enabled\" : true }" -u admin:admin http://localhost:8087/admin/bucket_server
```

7. Зарегестрировать конфигурацию: ws://ххх.ххх.ххх.ххх:9094 `ws://10.78.77.62:9094`
8. Зарегестрировать пользователей с помощью обработки

### Установка MINIO

https://dl.min.io/server/minio/release/windows-amd64/minio.exe

.\minio.exe server C:\minio --console-address :9090

http://192.0.2.10:9090

minioadmin
minioadmin


## Create bucket
Перейти `http//localhost:9000`
В интерфейсе Minio создать Bucket: `cs-bucket`

## Connect Minio to CS server

`sudo su - postgres`

`nano /tmp/create_bucket.sql`

insert in file - заменить `<ServerIP>` :

    INSERT INTO public.storage_server(id, 
								  type, 
								  base_url, 
								  container_url, 
								  container_name, 
								  region, 
								  access_key_id, 
								  secret_key, 
								  signature_version, 
								  is_deleted, 
								  upload_limit, 
								  download_limit, 
								  file_size_limit, 
								  created_at, 
								  updated_at, 
								  cdn_url, 
								  cdn_key_id, 
								  cdn_secret_key, 
								  state, 
								  cdn_enabled, 
								  path_style_access_enabled, 
								  bytes_to_keep, 
								  days_to_keep, 
								  pricing_url,
								 api_type,
								 storage_type)
	VALUES(
	uuid_generate_v4(), 'AMAZON', 'http://192.168.31.134:9000','http://192.168.31.134:9000/${container_name}',
	'cs-bucket',
	'',
	'minioadmin',
	'minioadmin',
	'V2', false, 1073741824, 1073741824, 104857600, CURRENT_TIMESTAMP, CURRENT_TIMESTAMP, NULL, NULL, NULL, 'ACTIVE', false, true, 0, 0, NULL, 'AMAZON', 'DEFAULT');


`psql -U postgres --dbname=cs_db --file=/tmp/create_bucket.sql`


Базу 1С подключаем по адресу: `ws://192.168.1.39:8087`





