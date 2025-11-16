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
</details>

TODO: UPLOAD YML TO THIS REPO

create nginx container for reverse proxy
```
sudo docker compose -f nginx.yml up -d
```

configure nginx
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
sudo docker compose -f bitwarden.yml up -d
```

https://www.google.com/search?q=raspberry+pi+vaultwarden+install&sca_esv=ad5d78fe25a26ce1&ei=hAIWacCfMqT_7_UPxdei0Qs&ved=0ahUKEwjA8by_we-QAxWk_7sIHcWrKLoQ4dUDCBE&uact=5&oq=raspberry+pi+vaultwarden+install&gs_lp=Egxnd3Mtd2l6LXNlcnAiIHJhc3BiZXJyeSBwaSB2YXVsdHdhcmRlbiBpbnN0YWxsMgYQABgWGB4yBRAAGO8FMgUQABjvBTIFEAAY7wVIhDZQvhpY5DRwBngBkAEAmAF5oAGcB6oBAzkuMrgBA8gBAPgBAZgCEaACwAfCAgoQABhHGNYEGLADwgIIEAAYBxgeGBPCAgcQABiABBgTwgIGEAAYHhgTwgIIEAAYCBgeGBPCAgoQABgIGB4YExgKwgIIEAAYFhgeGBOYAwCIBgGQBgiSBwQxNC4zoAfXI7IHAzguM7gHsgfCBwQwLjE3yAca&sclient=gws-wiz-serp
https://www.heise.de/select/ct/2021/9/2105509303473355898
https://getupnote.com/share/notes/caLALNnK5LMmACDgfkOdFLDxoSt1/1848505a-968b-49bb-b69d-d5bc6aa937ab
https://github.com/ttionya/vaultwarden-backup?tab=readme-ov-file#configure-and-check
