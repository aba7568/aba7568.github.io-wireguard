# aba7568.github.io-wireguard
# Setting Up
- create an ubuntu droplet after seeting a DO account 
- click console to get started 

# Making Sure Docker/compose is Installed 
``````bash 
docker --version 
docker-compose --version 

# Docker-compose was not isntalled so i had to install it 

sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-linux-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Run docker-compose --version again to make sure it is installed 
``````

# Setting Up WireGurad 
Create direcotry 
  ``````bash
  mkdir -p ~/wireguard/
  mkdir -p ~/wireguard/config/
  ``````
Create and add content to the docker compose file
  ``````bash
  nano ~/wireguard/docker-compose.yml
  #type in 
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_Kong
      - SERVERURL=64.23.138.60
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
# save and exit with ctrl + s & ctrl + x
``````
Start and Run 
`````bash
cd ~/wireguard/
docker-compose up -d
docker-compose logs -f wireguard
`````
Get the info in the config files  
```bash
cat peer_pc1.conf
```
## Next Steps 
- copy the info inot wireguard and activate 
