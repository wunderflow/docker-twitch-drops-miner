# docker-twitch-drops-miner

Based on this image: https://github.com/jlesage/docker-baseimage-gui

```
docker build -t dungfu/twitch-drops-miner:latest .
docker push dungfu/twitch-drops-miner:latest
```

Don't forget to mount `/TwitchDropsMiner/config` to have data persistence.

Docker-Compose
```
services:
  tmine:
    build:
      context: https://github.com/wunderflow/docker-twitch-drops-miner.git#main
      dockerfile: Dockerfile
    #fixes portainer pull button 
    pull_policy: build
    container_name: tmine
    hostname: tmine
    environment:
      - PUID=99
      - PGID=100
      - TZ=America/New_York
      #extra envs for baseinage-gui resolution
      - KEEP_APP_RUNNING=1
      - DISPLAY_WIDTH=1280
      - DISPLAY_HEIGHT=720
#ports not needed if using custom networks
    ports:
      - 5800:5800
    networks:
      - tmine_network
    volumes:
      - tmine_data:/TwitchDropsMiner/config
    deploy:
      resources:
        limits:
          #cpu limit @20%
          cpus: '0.2'
          memory: 750M
    restart: unless-stopped 
    
#need these lines if network/volumes exist already
#dont need for bind mounts
#networks:
#  tmine_network:
#    external: true
#
#volumes:
#  tmine_data:
#    external: true
```
