# Install PostreSQL
Add the PostgreSQL repository.

    sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

Add the PostgreSQL signing key.

    wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

Install PostgreSQL.

    sudo apt install postgresql postgresql-contrib -y
	
> For different version: `sudo apt install postgresql-12`

Enable the database server to start automatically on reboot.

    sudo systemctl enable postgresql
	
Start the database server.

    sudo systemctl start postgresql

Change the default PostgreSQL password.

    sudo passwd postgres

## Create User and Base

Switch to the postgres user.

    su - postgres

Create a user named sonar.

    createuser <UserName>

Log in to PostgreSQL.

    psql

Set a password for the sonar user. Use a strong password in place of my_strong_password.

    ALTER USER <UserName> WITH ENCRYPTED password '<UserPwd>';

Create a sonarqube database and set the owner to sonar.

    CREATE DATABASE <BaseName> OWNER <UserName>;

Grant all the privileges on the sonarqube database to the sonar user.

    GRANT ALL PRIVILEGES ON DATABASE <BaseName> to <UserName>;

Exit PostgreSQL.

    \q

Return to your non-root sudo user account.

    exit
	
	
# Install pgAdmin4

Setup the repository
Install the public key for the repository (if not done previously):

    sudo curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add

Create the repository configuration file:

    sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'

Install pgAdmin

    sudo apt install pgadmin4

In pgAdmin Tool
Password: `postgres`
Base: `Postgres`

## Traubleshooting 
https://interface31.ru/tech_it/2014/05/tipovye-oshibki-ustanovki-servera-1s-i-postgresql-na-platforme-linux.html
