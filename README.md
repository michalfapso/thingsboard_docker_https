# thingsboard_docker_https
Thingsboard monolithic (single server) setup with Let's Encrypt certificate served by Nginx reverse proxy.


## Motivation
The official tutorial for Thingsboard + HAProxy + Let's Encrypt is not dockerized: https://thingsboard.io/docs/user-guide/install/pe/add-haproxy-ubuntu/


## Usage
Follow the official tutorial to setup a simple Thingsboard-only service: https://thingsboard.io/docs/user-guide/install/docker/#detaching-stop-and-start-commands

Choose your preferred docker-compose.yml from this repository and make a copy:
```
cp docker-compose.yml.tb_swag_watchtower docker-compose.yml
```
Edit it to match your server setup. Set the `URL` parameter in the `swag` service's environment params in `docker-compose.yml` to match your server's url (e.g. mydomain.com) and make sure to have a wildcard DNS A record `*.mydomain.com` pointing to your server's IP address.

Run:
```
docker-compose up -d
```
After a minute or two, you should be able to load your Thingsboard webapp at https://tb.mydomain.com

**Warning:** Let's Encrypt has a limit of 5 requests per week, so if you plan to experiment with the configuration a bit, set `STAGING=true` in the `swag` service environment params.


## Description of docker-compose.yml files

- **docker-compose.yml.tb** contains the most simple configuration with only
the thingsboard image (without https). Source: https://thingsboard.io/docs/user-guide/install/docker/

- **docker-compose.yml.tb_swag** Thingsboard + SWAG
([linuxserver/swag](https://github.com/linuxserver/docker-swag) contains all info about Let's Encrypt parameters).
SWAG provides the Nginx reverse proxy with automatic Let's Encrypt certificate. The `nginx.thingsboard.subdomain.conf` file was modified from another SWAG config following [@mfalkvidd's](https://gist.github.com/mfalkvidd) config [here](https://gist.github.com/mfalkvidd/3920bd5035806a583b5c1e20eaddabae). It sets the `tb` subdomain to be redirected to the Thingsboard service.

- **docker-compose.yml.tb_swag_watchtower** Thingsboard + SWAG + Watchtower. Same as above plus automatic updates
of the SWAG image provided by [containerrr/watchtower](https://github.com/containrrr/watchtower).


## Troubleshooting
When I ran the compose for the first time, it started correctly, but when I restarted it, I got:
```
org.postgresql.util.PSQLException: Connection to localhost:5432 refused. Check that the hostname and port are correct and that the postmaster is accepting TCP/IP connections.
```
The solution found by [@pieromacaluso](https://github.com/pieromacaluso) in [this issue](https://github.com/thingsboard/thingsboard/issues/3439) was to run again the following:
```
sudo chown -R 799:799 ~/.mytb-data
sudo chown -R 799:799 ~/.mytb-logs
```
After this, Thingsboard started well everytime.
