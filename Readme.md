# Polr Docker Image

Made with Alpine, NGINX and PHP 7.

This is an unofficial Dockerimage for the [Polr URL Shortener](https://polrproject.org/).

## Usage

Simply run 

```bash
docker run -p 80:80 -d kolaente/polr
```

The Container needs another DB-Container which holds the database. You can use [MariaDB](https://hub.docker.com/_/mariadb/)/[Mysql](https://hub.docker.com/_/mysql/).

## Volumes

The container has one volume:
* `/var/session`. Holds PHP session data. Very useful when using a loadbalancer with mutliple instances of Polr.

## Docker-Compose

Probably the easiest way to run the image is by using [docker-compose](https://docs.docker.com/compose/):

**Note:** You really should change `MYSQL_ROOT_PASSWORD` when running in production!

```yaml
version: '2'
services:
  app:
    image: kolaente/polr
    restart: always
    ports:
      - "8080:80"
    depends_on:
      - db
    volumes: # Save the config
      - ./.env:/var/www/.env
  db:
    restart: always
    image: mariadb
    environment:
      - MYSQL_ROOT_PASSWORD=changeme #CHANGE THIS!
      - MYSQL_DATABASE=polr
    volumes:
      - ./db/:/var/lib/mysql
```

Afterwords, install Polr by pointing your browser to `server_url/setup` and input the nessesary fields. For DB-Host, use `db`

## Contributing

If you run into any issues with the image or discover a bug, feel free to [create a new issue](https://github.com/kolaente/polr-docker/issues/new) on Github.

Or, if you have any improvements to the image, fork and create a pull request.

General help and feedback is below in the comments.
