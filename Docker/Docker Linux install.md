# Installation
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

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