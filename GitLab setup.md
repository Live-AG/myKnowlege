## Installation from

https://about.gitlab.com/install/#ubuntu

    sudo apt update
    sudo apt install -y curl openssh-server ca-certificates tzdata perl

---

    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash

---

    sudo apt install gitlab-ee

---

    sudo ufw allow http
    sudo ufw allow https
    sudo ufw allow OpenSSH

    sudo ufw status


edit file

    sudo nano /etc/gitlab/gitlab.rb

set in file

    external_url 'https://example.com'

then

    sudo gitlab-ctl reconfigure

to change root password

    sudo gitlab-rake "gitlab:password:reset[root]"


# Goto Sleep
