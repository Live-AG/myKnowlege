1. Установить Liberica JDK 16 (bellsoft-jdk16.0.1+9-windows-amd64-full)
2. Установить PostgresSQL (postgresql-13.3-2-windows-x64) 
	2.1 Создать пользователя сервера: db_user (с правами CREATEDB) (Пароль: admin)
	
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
		  
	2.2 Создать базу: cs_db
		-- Database: cs_db
		-- DROP DATABASE cs_db;
		
		CREATE DATABASE cs_db
		    WITH 
		    OWNER = db_user
		    ENCODING = 'UTF8'
		    LC_COLLATE = 'Russian_Russia.utf8'
		    LC_CTYPE = 'Russian_Russia.utf8'
		    TABLESPACE = pg_default
		    CONNECTION LIMIT = -1;
	
	2.3 Для базы выполнить на SQL:
		CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
		
3. Установить Сервер взаимодействия (1ce-installer от администратора)
4. Выполнить в CMD:

		ring hazelcast instance create --dir C:\hazelcast
		ring elasticsearch instance create --dir C:\elasticsearch
		ring cs instance create --dir C:\cs

		ring cs --instance cs jdbc pools --name common set-params --url jdbc:postgresql://localhost:5432/cs_db?currentSchema=public
		ring cs --instance cs jdbc pools --name common set-params --username db_user
		ring cs --instance cs jdbc pools --name common set-params --password admin
		ring cs --instance cs jdbc pools --name privileged set-params --url jdbc:postgresql://localhost:5432/cs_db?currentSchema=public
		ring cs --instance cs jdbc pools --name privileged set-params --username db_user
		ring cs --instance cs jdbc pools --name privileged set-params --password admin

		ring hazelcast --instance hazelcast service create
		ring elasticsearch --instance elasticsearch service create
		ring cs --instance cs service create

5. Выполнить настройку

		curl -Sf -X POST -H "Content-Type: application/json" -d "{ \"url\" : \"jdbc:postgresql://localhost:5432/cs_db\", \"username\" : \"db_user\", \"password\" : \"admin\", \"enabled\" : true }" -u admin:admin http://localhost:8087/admin/bucket_server

6. Проверить работу сервера: http://localhost:8087/rs/health

7. Зарегестрировать конфигурацию:
	ws://ххх.ххх.ххх.ххх:8087
8. Зарегестрировать пользователей
9. Для настройки файлового хранилища MIMO см. https://its.1c.ru/db/metod8dev/content/5982/hdoc


# Minio installation
https://www.8host.com/blog/nastrojka-xranilishha-obektov-s-pomoshhyu-minio-v-ubuntu-18-04/

Логин и Пароль (по умолчанию): minioadmin:minioadmin

После установки Minio выполнить запрос в Postgres:

	INSERT INTO public.storage_server(id, type, base_url, container_url, container_name, region, access_key_id, secret_key, signature_version, is_deleted, upload_limit, download_limit, file_size_limit, created_at, updated_at, cdn_url, cdn_key_id, cdn_secret_key, state, cdn_enabled, path_style_access_enabled, bytes_to_keep, days_to_keep, pricing_url)
	VALUES(
	uuid_generate_v4(), 'AMAZON', 'http://10.78.77.62:9000','http://10.78.77.62:9000/${container_name}',
	'cs-bucket',
	'',
	'minioadmin',
	'minioadmin',
	'V2', false, 1073741824, 1073741824, 104857600, CURRENT_TIMESTAMP, CURRENT_TIMESTAMP, NULL, NULL, NULL, 'ACTIVE', false, true, 0, 0, NULL);
