# Setup Raspberry pi:

- run Raspberry Pi Imager [https://www.raspberrypi.com/software/]
- set user
- enable SSH

# init Raspberry:

update system

```
sudo apt update && sudo apt upgrade -y && sudo apt autoclean && sudo reboot
```

install docker
```
sudo apt install docker.io -y
```

docker autostart on boot
```
sudo systemctl enable --now docker
```

install docker compose
```
sudo apt install docker-compose -y
```
# vaultwarden in docker

create folder
```
sudo mkdir /home/pi/docker/nginx # put your yml file here
sudo mkdir /home/pi/docker/bitwarden # put your yml file here
sudo mkdir /srv/bitwarden # create this to avoid error on bind-mounted volumes
```

<details>
  <summary>setup backup</summary>
  
create rclone config

```yml
docker run --rm -it \
   --mount type=volume,source=bitwarden-rclone-config,target=/config/ \
   ttionya/vaultwarden-backup:latest \
   rclone config

n) New remote
Name: BitwardenBackup
Type of storage: 3 (alias)

# It's the path WITHIN the container
path to alias: /data
```

show created config

```yml
  docker run --rm -it \
     --mount type=volume,source=bitwarden-rclone-config,target=/config/ \
     ttionya/vaultwarden-backup:latest \
     rclone config show 
```

restore command

```
sudo mkdir -p /srv/bitwarden-restore
sudo cp /pfad/zu/backup.20250121.zip /srv/bitwarden-restore/
cd /srv/bitwarden-restore

sudo docker run --rm -it \
--mount type=bind,source=/srv/bitwarden/data,target=/data/ \
--mount type=bind,source=$(pwd),target=/bitwarden/restore/ \
-e DATA_DIR="/data" \
 ttionya/vaultwarden-backup:latest restore \
--zip-file backup.20250121.zip \
--password 'WHEREISMYPASSWORD'
```
</details>

TODO: UPLOAD Dockerfile TO THIS REPO

create own certbot image based on official certbot image and add a additional command

```
docker build -t my-certbot-strato:latest .
```

TODO: UPLOAD YML TO THIS REPO

create nginx container for reverse proxy
```
sudo docker compose -f nginx.yml up -d
```

### Automated Letsencrypt Certificates - Strato as a DNS Provider
Strato doesn't offer an official API for DNS, which makes fully automated DNS-01 solutions somewhat tricky. However, there are community projects like `certbot-dns-strato` that log into the Strato customer center and set the TXT records for you, allowing you to automate DNS-01.

Alternatively, you can switch your domain's nameservers to a DNS provider with an official ACME API (e.g., deSEC, Cloudflare, Hetzner DNS) and use an official DNS plugin type from Certbot or acme.sh.

copy strato.ini for DNS-01 acme challenge to `/srv/letsencrypt/strato.ini` (so it is accessible by the container on `etc/letsencrypt/strato.ini`)

set permission very restrictiv because it contains passwords
```
sudo chmod 600 /srv/letsencrypt/strato.ini
```

copy `domain.txt` file
copy `request-cert.sh`

run certification script. The command creates a certification per domain. It has to be done only once because after it `renew` will take place
```
chmod +x /srv/request-cert.sh
/srv/request-cert.sh
```

### configure nginx

we need to copy the default config of nginx because we bind-mounted `/etc/nginx/conf.d/` and this prevents nginx to create any files in its config folder because "it is managed by the host system".

TODO: UPLOAD DEFAULT CONFIGS TO THIS REPO
TODO: UPLOAD DEFAULT index.html TO THIS REPO

by default you can't copy files to `/srv`. Change permissions and then copy it (e.g. with WinSCP)
```
sudo chmod 777 /srv/nginx/conf
sudo chmod 777 /srv/nginx/html
```

TODO: UPLOAD YML TO THIS REPO

create bitwarden container
```
sudo mkdir -p /srv/bitwarden/data
sudo docker compose -f bitwarden.yml up -d
```
