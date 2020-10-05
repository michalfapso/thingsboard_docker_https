# thingsboard_docker_https
Thingsboard monolithic (single server) setup with Let's Encrypt certificate served by Nginx reverse proxy.


## Motivation
The official tutorial for Thingsboard + HAProxy + Let's Encrypt is not dockerized: https://thingsboard.io/docs/user-guide/install/pe/add-haproxy-ubuntu/


## Usage
Choose your preferred docker-compose.yml, edit it to match your setup and run `docker-compose`

e.g.: `docker-compose -f docker-compose.yml.tb_swag_watchtower`


## Description of docker-compose.yml files

- **docker-compose.yml.tb** contains the most simple configuration with only
the thingsboard image (without https). Source: https://thingsboard.io/docs/user-guide/install/docker/

- **docker-compose.yml.tb_swag** Thingsboard + SWAG
([linuxserver/swag](https://github.com/linuxserver/docker-swag) contains all info about Let's Encrypt parameters).
SWAG provides the Nginx reverse proxy with automatic Let's Encrypt certificate

- **docker-compose.yml.tb_swag_watchtower** Thingsboard + SWAG + Watchtower. Same as above plus automatic updates
of the SWAG image provided by [containerrr/watchtower](https://github.com/containrrr/watchtower).
