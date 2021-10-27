## Source

https://losst.ru/ustanovka-servera-1s-na-ubuntu-20-04

>ВНИМАНИЕ! Установка без HASP


### Install Curl
	
	sudo apt -y install mc curl
	
### Adding repository PostgreSQL PRO

	sudo curl -o apt-repo-add.sh https://repo.postgrespro.ru/pgpro-13/keys/apt-repo-add.sh
	sudo sh apt-repo-add.sh
	
## Install PostgresSQL PRO STD

#### Добавить поддержку русского языка в систему:

	sudo locale-gen en_US.UTF-8
	sudo locale-gen ru_RU.UTF-8
	sudo update-locale LANG=ru_RU.UTF8
	sudo dpkg-reconfigure locales
	
### Setup PostgreSQL

	sudo apt -y install postgrespro-std-13 postgrespro-std-13-contrib

Следующие комманды выполнять от пользователя root

	sudo su root

Удалить базу по умолчанию и добавим новую для 1С

	systemctl stop postgrespro-std-13
	rm -r /var/lib/pgpro/std-13/data/*
	/opt/pgpro/std-13/bin/pg-setup initdb --tune=1c
	
Настроить службу	

	systemctl enable postgrespro-std-13
	systemctl start postgrespro-std-13
	systemctl status postgrespro-std-13
	
	
#### Setup access to DataBase
устанавливить пароль `postgres` для пользователя `postgres` 

	su postgres
	psql
	alter user postgres with encrypted password 'postgres';
	\q
	exit
	
