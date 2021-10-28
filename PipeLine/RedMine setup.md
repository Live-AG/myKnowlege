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

    sudo apt install libapache2-mod-passenger
    /etc/init.d/apache2 reload


    sudo gem update
    sudo gem install bundler

edit file: `/etc/apache2/mods-available/passenger.conf`
with

    <IfModule mod_passenger.c>
        PassengerDefaultUser www-data
        PassengerRoot /usr
        PassengerRuby /usr/bin/ruby
    </IfModule>

Now create a symlink to connect Redmine into the web document space:

    sudo ln -s /usr/share/redmine/public /var/www/html/redmine

Edir file: `/etc/apache2/sites-available/000-default.conf`
with

    <Directory /var/www/html/redmine>
        RailsBaseURI /redmine
        PassengerResolveSymlinksInDocumentRoot on
    </Directory>

---

    sudo touch /usr/share/redmine/Gemfile.lock
    sudo chown www-data:www-data /usr/share/redmine/Gemfile.lock

Now restart apache:

    sudo service apache2 restart

---

We're sorry, but something went wrong.



