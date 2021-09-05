# Install PostgreSQL
    sudo apt update
    sudo apt install postgresql postgresql-contrib

# Connecting to consol PostgreSQL:

    sudo -u postgres psql
    \password
    
-> `postgres` -> `postgres`

    CREATE USER rm_user WITH PASSWORD 'postgres'; CREATE DATABASE myredminedb;GRANT ALL PRIVILEGES ON DATABASE myredminedb to rm_user;
    
    exit

# Install RedMine

    sudo apt install redmine redmine-pgsql


