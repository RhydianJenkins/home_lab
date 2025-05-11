# Add media mount

```
# add to home/DiskStation creds to ~/.mnt-credentials
username=rhydian
password=password

# add to /etc/fstab (update local address to nas address
//192.168.4.54/media /mnt/media cifs credentials=/home/rhydian/.mnt-credentials 0 0
```

# Start containers

```
docker compose up -d
```
