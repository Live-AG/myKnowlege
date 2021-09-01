## Installation from

https://about.gitlab.com/install/#ubuntu


### Install and configure the necessary dependencies

    sudo apt-get update
    sudo apt-get install -y curl openssh-server ca-certificates tzdata perl

### Add the GitLab package repository and install the package

Add the GitLab package repository.

    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash

Next, install the GitLab package.

    sudo EXTERNAL_URL="https://gitlab.example.com" apt-get install gitlab-ee

### Edit file

    sudo nano /etc/gitlab/gitlab.rb

with

    external_url 'http://gitlab.local'

then 

    sudo gitlab-ctl reconfigure

if включён брандмауэр, необходимо добавить в исключения порты для протоколов http и ssh:

    sudo ufw allow ssh
    sudo ufw allow http

Чтобы наш локальный домен работал, необходимо добавить запись о нём в файл /etc/hosts:

    sudo nano /etc/hosts

add in file

    127.0.0.1 gitlab.local


### Apache

    sudo nano /etc/gitlab/gitlab.rb

add in file

    external_url 'http://gitlab.local'
    nginx['enable'] = false
    web_server['external_users'] = ['www-data']

then

    sudo gitlab-ctl reconfigure

### Apache installation

    sudo apt install apache2

# Goto Sleep
