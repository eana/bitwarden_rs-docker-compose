# Bitwarden_rs docker-compose

This micro-repo is used only as a host for an easily hostable `bitwarden_rs`
config.

# Use it

Create a new `docker-compose.secrets.yml` with the following content:

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
