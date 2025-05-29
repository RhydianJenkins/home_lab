# Install docker

[Instructions](https://docs.docker.com/engine/install/debian/) can be found on the official docker site.

```sh
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# To install the latest version
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Create docker group
sudo groupadd docker

# Add your user to the docker group
sudo usermod -aG docker $USER

# Log out and in again to apply changes, or possibly run
newgrp docker

# Verify the installation
sudo docker run hello-world
```

# Add media mount

```md
# add creds to ~/.mnt-credentials
username=rhydian
password=password # replace me with home/DiskStation creds!

# add to /etc/fstab (update local address to nas address)
//192.168.4.54/media /mnt/media cifs credentials=/home/rhydian/.mnt-credentials,uid=1000,gid=1000,iocharset=utf8,file_mode=0664,dir_mode=0775 0 0
```

# Tell docker to wait for media mount

```
$ cat /etc/systemd/system/docker.service.d/wait-for-mounts.conf

[Unit]
RequiredMountsFor=/mnt/media
```

# Populate .env

```sh
cp .env.example .env # don't forget to populate these!
```

# Generate SSL certificates

Generates certificates in `/etc/letsencrypt` to be mounted into the nginx container.

```sh
sudo certbot certonly --standalone -d example.com -d jellyfin.example.com
```

# Start containers

```
docker compose up -d
```
