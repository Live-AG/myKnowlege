
## Summary

Password: `1qaz@WSX`

## Installation from

https://about.gitlab.com/install/#ubuntu

    sudo apt update
    sudo apt install -y curl openssh-server ca-certificates tzdata perl

---

    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash

---
### To install Enterprise Edition
    sudo apt install gitlab-ee

### To install Community Edition
    sudo apt install gitlab-ce

---

    sudo ufw allow http
    sudo ufw allow https
    sudo ufw allow OpenSSH
    sudo ufw enable

    sudo ufw status

edit file

    sudo nano /etc/gitlab/gitlab.rb

set in file

    external_url 'http://gitlab.example.com'

if needed to another port change

    external_url 'http://gitlab.example.com:9999'

then

    sudo gitlab-ctl reconfigure

If porn changed

    sudo ufw allow 9999

to change root password

    sudo gitlab-rake "gitlab:password:reset[root]"

then check in browser: `http://localhost:9999'

# Goto Sleep

## TroubleShooting



#### PUMA error
```
Address already in use - \
bind(2) for "127.0.0.1" port 3000 (Errno::EADDRINUSE)
```

https://github.com/puma/puma/issues/1785


![[Pasted image 20220512170152.png]]

	sudo nano /etc/gitlab/gitlab.rb

в файле установить порт:

	puma['port'] = 3005