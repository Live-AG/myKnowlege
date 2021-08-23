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

5. Проверить работу сервера: http://localhost:8087/rs/health
6. Выполнить настройку
curl -Sf -X POST -H "Content-Type: application/json" -d "{ \"url\" : \"jdbc:postgresql://localhost:5432/cs_db\", \"username\" : \"db_user\", \"password\" : \"admin\", \"enabled\" : true }" -u admin:admin http://localhost:8087/admin/bucket_server

7. Зарегестрировать конфигурацию:
	ws://ххх.ххх.ххх.ххх:8087
8. Зарегестрировать пользователей
9. Для настройки файлового хранилища MIMO см. https://its.1c.ru/db/metod8dev/content/5982/hdoc


***MINIO***
https://www.8host.com/blog/nastrojka-xranilishha-obektov-s-pomoshhyu-minio-v-ubuntu-18-04/



*******************************LINUX********************************

awk: line 2: function gensub never defined !!!!!!!!!!!!!!!!!

//********************************** Info source **********************************
https://nizamov.school/ustanovka-servera-vzaimodejstviya-1s-na-ubuntu-server-20-04/
//*********************************************************************************

cd /opt/1C/1CE/components/1c-enterprise-ring-0.19.5+12-x86_64

sudo ./ring cs instance create --dir /var/cs/cs_instance --owner cs_user

sudo ./ring cs instance create --dir /var/cs/cs_instance --owner cs_user
sudo ./ring cs --instance cs_instance service create --username cs_user --java-home JAVA_HOME --stopped
sudo ./ring cs instance create --dir /var/cs/cs_instance --owner cs_user


postgres

sudo ./ring hazelcast instance create --dir /var/cs/hc_instance --owner hc_user
sudo ./ring hazelcast --instance hc_instance service create --username hc_user --java-home JAVA_HOME --stopped

sudo ./ring elasticsearch instance create --dir /var/cs/elastic_instance --owner elastic_user
sudo ./ring elasticsearch --instance elastic_instance service create --username elastic_user --java-home JAVA_HOME --stopped

sudo ./ring cs --instance cs_instance jdbc pools --name common set-params --url jdbc:postgresql://localhost:5432/cs_db?currentSchema=public
sudo ./ring cs --instance cs_instance jdbc pools --name common set-params --username postgres
sudo ./ring cs --instance cs_instance jdbc pools --name common set-params --password postgres
sudo ./ring cs --instance cs_instance jdbc pools --name privileged set-params --url jdbc:postgresql://localhost:5432/cs_db?currentSchema=public
sudo ./ring cs --instance cs_instance jdbc pools --name privileged set-params --username postgres
sudo ./ring cs --instance cs_instance jdbc pools --name privileged set-params --password postgres

sudo ./ring cs --instance cs_instance websocket set-params --hostname 192.168.1.39
sudo ./ring cs --instance cs_instance websocket set-params --port 8087

sudo ./ring hazelcast --instance hc_instance service start
sudo ./ring elasticsearch --instance elastic_instance service start
sudo ./ring cs --instance cs_instance service start

sudo apt update
sudo apt install curl

sudo curl -Sf -X POST -H "Content-Type: application/json" -d "{ \"url\" : \"jdbc:postgresql://localhost:5432/cs_db\", \"username\" : \"postgres\", \"password\" : \"postgres\", \"enabled\" : true }" -u admin:admin http://localhost:8087/admin/bucket_server

firewall-cmd --reload
firewall-cmd --zone=public --add-port=8087/tcp --permanent


#FILE STORAGE SETUP

 sudo mkdir -p /opt/minio
 sudo chown user:user /opt/minio
 sudo mkdir -p /var/minio
 sudo chown user:user /var/minio
 cd /opt/minio
 wget https://dl.min.io/server/minio/release/linux-amd64/minio
 sudo chmod +x minio
 sudo MINIO_ROOT_USER=admin MINIO_ROOT_PASSWORD=12345678 ./minio server /mnt/data --console-address ":9001"