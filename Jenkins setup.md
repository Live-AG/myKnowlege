# Jenkins install

6hyYUbyNwkxDF6yfxqy5

[[Java11 Install]]

Add repository key

    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

Set repository address

    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

Install

    sudo apt update
    sudo apt install jenkins

# Configure service

    sudo systemctl start jenkins
    sudo systemctl status jenkins

# Configure Firewall

    sudo ufw allow 8080
    sudo ufw allow OpenSSH
    sudo ufw enable

# TroubleShooting

To analise trouble see logs at

    sudo nano /var/log/jenkins/jenkins.log

If doesnt start in web - change addres and/or port

    sudo nano /etc/default/jenkins

for examlpe:

>HTTP_HOST=192.168.1.106
>HTTP_PORT=8081

