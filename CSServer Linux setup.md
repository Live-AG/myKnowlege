
# Links

https://nizamov.school/ustanovka-servera-vzaimodejstviya-1s-na-ubuntu-server-20-04/


# JAVA Install

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


