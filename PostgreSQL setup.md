Add the PostgreSQL repository.

    $ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

Add the PostgreSQL signing key.

    $ wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

Install PostgreSQL.

    $ sudo apt install postgresql postgresql-contrib -y

Enable the database server to start automatically on reboot.

    $ sudo systemctl enable postgresql

Start the database server.

    $ sudo systemctl start postgresql

Change the default PostgreSQL password.

    $ sudo passwd postgres

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