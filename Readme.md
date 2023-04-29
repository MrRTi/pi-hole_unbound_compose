# Pi-hole with Unbound in docker

## Info

### Pi-hole
- https://docs.pi-hole.net
- https://github.com/pi-hole/docker-pi-hole

### Unbound
- https://docs.pi-hole.net/guides/dns/unbound/

### Prometheus exporter
- https://github.com/eko/pihole-exporter/

Compose file includes prometheus exporter for pihole metrics. It won't run by default `up` docker command

### Block lists
- https://firebog.net


## Prerequirements

You need the following software to be installed on your system:

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/)
- [Git](https://gitlab.com/help/topics/git/index.md)

## How to use

### Prepare

Download this setup to your host. Change `~/pi-hole_unbound_compose` to the path you needed

```
git clone https://github.com/MrRTi/pi-hole_unbound_compose.git ~/pi-hole_unbound_compose
```

Go into cloned folder

Copy the `.env.template` into `.env` and set you variables in `.env` file

```bash
cp .env.template .env
```

You will need to setup
- `PIHOLE_DOMAIN_NAME` - donain name used in compose setup for pihole service 
- `PIHOLE_WEBPASSWORD` - password for pihole web interface
- `REV_SERVER_DOMAIN` - If conditional forwarding is enabled, set the domain of the local network router
- `PIHOLE_API_TOKEN` - Used by prometheus exporter, could be set up after pi-hole run 

You could add additional variables supported by Pihole docker image. 
Check https://github.com/pi-hole/docker-pi-hole for the list of available variables

Image version are controlled by variables:
- `PIHOLE_VERSION`
- `PIHOLE_EXPORTER_VERSION`

### Build image

```bash
docker compose build
docker-compose build # for old docker compose versions
```

### Run Pi-hole

```bash
docker compose up
docker-compose up # for old docker compose versions
```

Use `-d` flag to run detached

You could access pi-hole at `http://localhost:PIHOLE_WEBPORT/admin/login.php` or `http://localhost/admin/login.php` if you didn't change the variable

Pi-hole will run using unbound, no additional setup needed.

Container will start if your host reboots. You could change this setting in docker-compose file
- https://docs.docker.com/compose/compose-file/compose-file-v3/#restart

You could change config files in config folder if you need.

At this point you could proceed with pihole setup at your network as described in pi-hole documentation

### Run pihole with prometheus exporter

```bash
docker compose --profile prometheus up
docker-compose --profile prometheus up # for old docker compose versions
```

### Stop pi-hole

```bash
docker compose stop
docker-compose stop # for old docker compose versions
```

### Stop pi-hole with prometheus exporter 

```bash
docker compose --profile stop
docker-compose --profile stop # for old docker compose versions
```

## Fresh start

Pihole data stored in docker volume and will be consistent between runs

If you want to remove everything and setup from scrath, run

```bash
docker compose --profile prometheus down --volumes
docker-compose --profile prometheus down --volumes # for old docker compose versions
```

## Logs

Logs are mapped to your host machine and will be saved at logs folder
- `./log/lighttpd`
- `./log/pihole`
 
