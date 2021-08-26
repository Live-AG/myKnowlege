
# Links

https://nizamov.school/ustanovka-servera-vzaimodejstviya-1s-na-ubuntu-server-20-04/

# JAVA Install

Instalation of Liberica JDK 11 `https://libericajdk.ru/pages/downloads/#/java-11-lts`

or

`sudo apt update`

`sudo apt install openjdk-11-jdk`

`java -version`

go to home directory

`cd ~`

`sudo nano .bashrc`

set line at the end of file: `export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64`

`source .bashrc`

`echo $JAVA_HOME`

# Install curl

`sudo apt update`

`sudo apt install curl`

# Install Postgres

`sudo apt-get update`

`sudo apt-get install postgresql postgresql-contrib`

Подключаемся к консоли PostgreSQL

`sudo -u postgres psql`

Устанавливаем пароль пользователю postgres. Я оставляю стандартный пароль, так как при инициализации базы данных ниже, мне не удалось передать сложный пароль в конфиг.

`\password postgres`
`postgres`

Создаем базу данных, устанавливаем расширение и выходим

`CREATE DATABASE cs_db;`
`\c cs_db`
`CREATE EXTENSION IF NOT EXISTS "uuid-ossp";`
`\q`

# Install pgAdmin 4

Setup the repository
Install the public key for the repository (if not done previously):

`sudo curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add`

Create the repository configuration file:

`sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt`

`update'`

Install pgAdmin

`sudo apt install pgadmin4`

In pgAdmin Tool
Password: `postgres`
Base: `Postgres`

# Установка сервер взаимодействия 1С
Download CS server: `https://releases.1c.ru/version_files?nick=CollaborationSystem&ver=10.0.47`
Go to 1CS catalog (exemple: `/home/andrey/Downloads/1c_cs_10.0.47_linux_x86_64`)

`sudo ./1ce-installer`

Создаем пользователя, необходимые папки и назначаем права.

`sudo useradd cs_user`

`sudo mkdir -p /var/cs/cs_instance`

`sudo chown cs_user:cs_user /var/cs/cs_instance`

С помощью утилиты ring создаем экземпляр сервера взаимодействия

`cd /opt/1C/1CE/components/1c-enterprise-ring-0.19.5+12-x86_64`

`sudo ./ring cs instance create --dir /var/cs/cs_instance --owner cs_user`

`sudo ./ring cs --instance cs_instance service create --username cs_user --java-home $JAVA_HOME --stopped`

Устанавливаем инстанс Hazelcast

`sudo useradd hc_user`

`sudo mkdir -p /var/cs/hc_instance`

`sudo chown hc_user:hc_user /var/cs/hc_instance`

`sudo ./ring hazelcast instance create --dir /var/cs/hc_instance --owner hc_user`

`sudo ./ring hazelcast --instance hc_instance service create --username hc_user --java-home $JAVA_HOME --stopped`

Устанавливаем инстанс Elasticsearch

`sudo useradd elastic_user`

`sudo mkdir -p /var/cs/elastic_instance`

`sudo chown elastic_user:elastic_user /var/cs/elastic_instance`

`sudo ./ring elasticsearch instance create --dir /var/cs/elastic_instance --owner elastic_user`

`sudo ./ring elasticsearch --instance elastic_instance service create --username elastic_user --java-home $JAVA_HOME --stopped`

Настройка JDBC

`sudo ./ring cs --instance cs_instance jdbc pools --name common set-params --url jdbc:postgresql://localhost:5432/cs_db?currentSchema=public`

`sudo ./ring cs --instance cs_instance jdbc pools --name common set-params --username postgres`

`sudo ./ring cs --instance cs_instance jdbc pools --name common set-params --password postgres`

`sudo ./ring cs --instance cs_instance jdbc pools --name privileged set-params --url jdbc:postgresql://localhost:5432/cs_db?currentSchema=public`

`sudo ./ring cs --instance cs_instance jdbc pools --name privileged set-params --username postgres`

`sudo ./ring cs --instance cs_instance jdbc pools --name privileged set-params --password postgres`

Настройка WebSocket сервера взаимодействия 1С

Порт можете оставить такой же 8087, а ip устанавливайте от вашего сервера, у меня это *10.10.1.67*

`sudo ./ring cs --instance cs_instance websocket set-params --hostname 192.168.1.39`

`sudo ./ring cs --instance cs_instance websocket set-params --port 8087`

Запускаем службы (from `cd /opt/1C/1CE/components/1c-enterprise-ring-0.19.5+12-x86_64`)

`sudo ./ring hazelcast --instance hc_instance service start`

`sudo ./ring elasticsearch --instance elastic_instance service start`

`sudo ./ring cs --instance cs_instance service start`

Проверяем, что все запустилось: `sudo curl http://localhost:8087/rs/health`

# Go to sleep
