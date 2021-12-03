# Install JAVA

    sudo apt update
    sudo apt install openjdk-11-jdk
    java -version

go to home directory

    cd ~
    sudo nano .bashrc

set line at the end of file: `export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64`

    source .bashrc
    echo $JAVA_HOME