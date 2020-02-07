# base traefik daemon

**Before starting up Traefik service, make sure that:**
- file `you_need.env` is renamed to `.env` and all ENV variables are correctly set

**There are two possible environments:**
- dev
- prod

### For PRODuction environment don't forget to set `.htpasswd` file.
Add passwords file:

`htpasswd -cb .htpasswd login pass`

Update passwords file:

`htpasswd -b .htpasswd login pass`

### Example

You are willing to run your cool service behind Traefik daemon. In order to connect your service to Traefik network and set up some routing rules you have to add some labels to your service's `docker-compose.yml`.

Example below shows `docker-compose.yml` of [whoami service](https://hub.docker.com/r/containous/whoami) with https support and redirect mechanism.

```
version: '3'

services:
  whoami:
    image: "containous/whoami"
    container_name: "whoami-service"
    labels:
      - "traefik.enable=true"
      - 'traefik.http.routers.whoami-secure.rule=Host("who.localhost")'
      - 'traefik.http.routers.whoami-secure.tls.certresolver=httpchallenge_0'
      - 'traefik.http.routers.whoami-http.rule=Host("who.localhost")'
      - 'traefik.http.routers.whoami-http.middlewares=redirect_middleware'
      - 'traefik.http.middlewares.redirect_middleware.redirectscheme.scheme=https'
    networks:
      - traefik

networks:
  traefik:
    external:
      name: traefik_network
```
