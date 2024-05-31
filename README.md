# Terraria Server Docker
this project is a wrapper for Terraria server to be deployed as Docker container

## Deployment with Docker Compose
```yaml
version: '3.9'

services:
  terraria:
    ports:
      - "7777:7777" # default port
    image: maha90/terraria:latest
    environment:
      - PORT=7777                       # default port
      - PASSWORD=$PASSWORD              # set the dserver password (no password if empty)
        #- AUTOCONFIG=2                 # auto generate world if not existing (difficulty based on terraria autocreate config)
        #- WORLD=/data/worlds/worldname # specify world name if not in config
        #- SEED=12345678                # seed for autogenerated
        #- WORLDNAME=Default            # world name if autogenerated
        #- DIFFICULTY=1                 # if autogenerated; based on terraria server config
        #- MAXPLAYERS=16                # max num of players allowed
        #- MOTD=Welcome to Terraria     # Message of the Day
        #- SAVE_INTERVAL=60             # interval between periodic saves in seconds
    volumes:
      - /local/path/serverconfig.txt:/data/config/serverconfig.txt # (optional) map a serverconfig
      - /local/path/worlds:/data/worlds
    container_name: terraria
```
## Capabilities
Most of the servers settings can be adjusted via environment variables. Settings also can be passed as 'serverconfig.txt' following the format for TerrariaServer. Environment variables have the higher priority.

If there is no world specified via 'serverconfig.txt' or environment variable the first world file in the '/data/worlds' directory is used. If there is no world file one will be automatically generated. the size of the world then will be defined by the 'AUTOCONFIG' environment variable (default: 2; medium).

When the container is stopped the server in the container will be gracefully stopped ('exit' command is sent to the server).

The world will be saved every x seconds, based on the 'SAVE_INTERVAL' variable.

## Sending commands to the server
Currently there is no direct interface to the Terraria Server. However all commands available for Terraria Server can be sent to it via the command:
```bash
docker exec -it terraria sh -c "echo 'COMMAND' > /tmp/terraria_input" # Where COMMAND is the command that should be forwareded
```
