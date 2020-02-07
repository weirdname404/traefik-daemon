# base traefik daemon

**Before starting up Traefik service, make sure that you have `.htpasswd` file.**

Add passwords file:

`htpasswd -cb .htpasswd login pass`

Update passwords file:

`htpasswd -b .htpasswd login pass`

### Example

Treafik settings in `docker-compose.yml` of your new service with https support and redirect mechanism will look something like this:

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
