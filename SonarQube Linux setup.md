
https://www.vultr.com/docs/install-sonarqube-on-ubuntu-20-04-lts

# Install JAVA

[[Java11 Install]]

  go to home directory

    cd ~
    sudo nano .bashrc

set line at the end of file: `export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64`

    source .bashrc

# Install PosgreSQL

[[PostgreSQL setup]]

## Create User and Base

Switch to the postgres user.

    su - postgres

Create a user named sonar.

    createuser sonar

Log in to PostgreSQL.

    psql

Set a password for the sonar user. Use a strong password in place of my_strong_password.

    ALTER USER sonar WITH ENCRYPTED password 'sonar';

Create a sonarqube database and set the owner to sonar.

    CREATE DATABASE sonarqube OWNER sonar;

Grant all the privileges on the sonarqube database to the sonar user.

    GRANT ALL PRIVILEGES ON DATABASE sonarqube to sonar;

Exit PostgreSQL.

    \q

Return to your non-root sudo user account.

    exit

# Download and Install SonarQube

nstall the zip utility, which is needed to unzip the SonarQube files.

    sudo apt-get install zip -y

Locate the latest download URL from the [[SonarQube official download]](https://www.sonarqube.org/downloads/) page.

Download the SonarQube distribution files.

    $ sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-<VERSION_NUMBER>.zip

> For example: sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.1.0.47736.zip

Unzip the downloaded file.

    sudo unzip sonarqube-<VERSION_NUMBER>.zip

> For example: sudo unzip sonarqube-9.1.0.47736.zip

Move the unzipped files to /opt/sonarqube directory

    sudo mv sonarqube-<VERSION_NUMBER> /opt/sonarqube

> For example: sudo mv sonarqube-9.1.0.47736 /opt/sonarqube

## Add SonarQube Group and User

Create a dedicated user and group for SonarQube, which can not run as the root user.

Create a sonar group.

    sudo groupadd sonar

Create a sonar user and set /opt/sonarqube as the home directory.

    sudo useradd -d /opt/sonarqube -g sonar sonar

Grant the sonar user access to the /opt/sonarqube directory.

    sudo chown sonar:sonar /opt/sonarqube -R

## Configure SonarQube

Edit the SonarQube configuration file.

    sudo nano /opt/sonarqube/conf/sonar.properties

Find the following lines:

    #sonar.jdbc.username=
    #sonar.jdbc.password=

Uncomment the lines, and add the database user and password you created in Step 2.

    sonar.jdbc.username=sonar
    sonar.jdbc.password=my_strong_password

Below those two lines, add the sonar.jdbc.url.

    sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube

Save and exit the file.

Edit the sonar script file.

    sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh

About 50 lines down, locate this line:

    #RUN_AS_USER=

Uncomment the line and change it to:

    RUN_AS_USER=sonar

Save and exit the file.


## Setup Systemd service

Create a systemd service file to start SonarQube at system boot.

    sudo nano /etc/systemd/system/sonar.service

Paste the following lines to the file.

    [Unit]
    Description=SonarQube service
    After=syslog.target network.target

    [Service]
    Type=forking

    ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
    ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

    User=sonar
    Group=sonar
    Restart=always

    LimitNOFILE=65536
    LimitNPROC=4096

    [Install]
    WantedBy=multi-user.target

Save and exit the file.

Enable the SonarQube service to run at system startup.

    sudo systemctl enable sonar

Start the SonarQube service.

    sudo systemctl start sonar

Check the service status.

    sudo systemctl status sonar

# Modify Kernel System Limits

SonarQube uses Elasticsearch to store its indices in an MMap FS directory. It requires some changes to the system defaults.

Edit the sysctl configuration file.

    sudo nano /etc/sysctl.conf

Add the following lines.

    vm.max_map_count=262144
    fs.file-max=65536
    ulimit -n 65536
    ulimit -u 4096

Save and exit the file.

Reboot the system to apply the changes.

    $ sudo reboot




