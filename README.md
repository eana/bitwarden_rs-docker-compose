# Bitwarden_rs docker-compose

This micro-repo is used only as a host for an easily hostable
[bitwarden_rs](https://github.com/dani-garcia/bitwarden_rs) config. We don't
want bitwarden to be accessible on all interfaces on the server, hence is
limited to the `docker0` interface. On the host machine bitwarden will be
accessible on `172.17.0.1:8100`.

To access bitwarden from outside you should have a reverse proxy in front of
it. Here are some [proxy
examples](https://github.com/dani-garcia/bitwarden_rs/wiki/Proxy-examples) that
could be used.

# How to run this

Clone this repository and create a new `docker-compose.secrets.yml` with the
following content:

```yaml
version: "3.3"
services:
  bitwarden-server:
    volumes:
      - /path/to/bw-data:/data
    environment:
      DATABASE_URL: 'mysql://<MARIADB_USER>:<MARIADB_PASSWORD>@bitwarden-mysql/bitwarden'
      DOMAIN: 'https://bitwarden.example.com'
  bitwarden-mysql:
    volumes:
      - /path/to/mysql/db:/var/lib/mysql
      - /path/to/mysql/export:/data
    environment:
      MARIADB_DATABASE: bitwarden
      MARIADB_USER: bitwarden
      MARIADB_PASSWORD: very-strong-password
      MARIADB_ROOT_PASSWORD: another-very-strong-password
```

After creating the file you can start bitwarden with:

```bash
docker-compose -f docker-compose.yml -f docker-compose.secrets.yml up -d
```
