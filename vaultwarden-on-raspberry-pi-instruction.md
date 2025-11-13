# Setup Raspberry pi:

- run Raspberry Pi Imager [https://www.raspberrypi.com/software/]
- set user
- enable SSH

# Init Raspi:

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
mkdir /home/pi/bitwarden
cd /home/pi/bitwarden
```

create compose file
```
nano bitwarden.yml
```

https://www.google.com/search?q=raspberry+pi+vaultwarden+install&sca_esv=ad5d78fe25a26ce1&ei=hAIWacCfMqT_7_UPxdei0Qs&ved=0ahUKEwjA8by_we-QAxWk_7sIHcWrKLoQ4dUDCBE&uact=5&oq=raspberry+pi+vaultwarden+install&gs_lp=Egxnd3Mtd2l6LXNlcnAiIHJhc3BiZXJyeSBwaSB2YXVsdHdhcmRlbiBpbnN0YWxsMgYQABgWGB4yBRAAGO8FMgUQABjvBTIFEAAY7wVIhDZQvhpY5DRwBngBkAEAmAF5oAGcB6oBAzkuMrgBA8gBAPgBAZgCEaACwAfCAgoQABhHGNYEGLADwgIIEAAYBxgeGBPCAgcQABiABBgTwgIGEAAYHhgTwgIIEAAYCBgeGBPCAgoQABgIGB4YExgKwgIIEAAYFhgeGBOYAwCIBgGQBgiSBwQxNC4zoAfXI7IHAzguM7gHsgfCBwQwLjE3yAca&sclient=gws-wiz-serp
https://www.heise.de/select/ct/2021/9/2105509303473355898
https://getupnote.com/share/notes/caLALNnK5LMmACDgfkOdFLDxoSt1/1848505a-968b-49bb-b69d-d5bc6aa937ab

```
```
```
```
```
```
