# Installation

https://docs.docker.com/engine/install/ubuntu/


	sudo apt-get update

# Pre-installation steps

	sudo apt-get install \
	    ca-certificates \
	    curl \
	    gnupg \
	    lsb-release

Add Docker’s official GPG key:


	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

set up the **stable** repository:


	echo \
	  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
	  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Installation

	sudo apt-get update
	sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin


# Post-installation steps

```
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker 
```

```
docker run hello-world
```

## Configure Docker to start on boot
```
 sudo systemctl enable docker.service
$ sudo systemctl enable containerd.service
```