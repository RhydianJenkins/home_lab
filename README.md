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
